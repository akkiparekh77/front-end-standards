# React performance Guidelines


## Intro

As a growing Frontend, expected more features coming ahead and pages being integrated with some legacy code as well.

This can be tricky as some legacy code use Functional components extensively and do not consider performance.

We found that many of our pages re-rendering for no reason with the same props.

It is important to track down any performance issue and fix it before merging into code, this will ensure that future integration/testing will be easier.

## Use React Functional component with Responsibility	

### Why is that important?
It is true that FC offers a lot of syntactic sugar and allows the use of hooks - for better data encapsulation/extraction - However,  we have to note that FC does some extra self-re-rendering comparing to a Classical component (class based component):
 
 *read [here](https://medium.com/takeaway-tech/a-deep-dive-into-pure-component-and-react-memo-and-why-do-we-need-them-ae99dce6a33c),  [here](https://reactjs.org/docs/components-and-props.html)* and [here](https://medium.com/wix-engineering/two-mistakes-in-react-js-we-keep-doing-over-and-over-again-b1aea20fb3f0) (very important!)
 
 As you probably aware by now: Sometimes there are absolutely no ways to prevent that, here another example:

Example that illustrate the issue:


```
import React, {useState, Fragment} from 'react';
import logo from './logo.svg';
import './App.css';

const WelcomeMessage = ({ name, parent }) => {
  console.log("render WelcomeMessage");
  return (<section>
      <div>Hello {name}</div>
      <div>Your Father {parent.father}</div>
      <div>Your Mother {parent.mother}</div>
    </section>);
}

const App = () => {
  console.log("render App");

  const initState = {
    name: "unnamed",
    age: 42,
    parent: {
      father: "-",
      mother: "Elissa"
    }
  }

  const [user, setUser] = useState(initState)

  const {name, parent} = user
  return (
    <Fragment>
      <button onClick={() => setUser({...user, name: 'Mr X'})}>
        Set new name "Mr X"
      </button>
      <button onClick={() => setUser({...user, parent: {...user.parent, father: 'Georg' }})}>
        Set new Father "Georg"
      </button>
      <WelcomeMessage 
        name={name} 
        parent={parent} 
      />
    </Fragment>
  );
}
 

export default App;
```


Setting the name 3 times, costs the app to make 3 full re-render (parent & child)

**Note**: The green/orange bars that are shown in the sreenshots are actual [commits](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html#browsing-commits) (The commit phase is when React applies any changes)

![Screenshot 2020-09-11 at 15 15 08](https://user-images.githubusercontent.com/7538862/92929978-b1b98b00-f441-11ea-9f60-1f7116fb9eb3.png)


It happens because : 

- The main component is comparing Object reference instead of the actual values
- Also because Hooks are changing on the main Components, they will automatically re-render all child components (by default)


## One of the solutions


```

import React, { Fragment, PureComponent } from 'react';
import logo from './logo.svg';
import './App.css';

const WelcomeMessage = React.memo(({ name, parent }) => {
  console.log("render WelcomeMessage");
  return (<section>
      <div>Hello {name}</div>
      <div>Your Father {parent.father}</div>
      <div>Your Mother {parent.mother}</div>
    </section>);
}, (prevProps, nextProps) => {
  console.log(prevProps, nextProps)
  const prevParent = prevProps.parent
  const nextParent = nextProps.parent
  return (
    prevParent.father === nextParent.father 
    && prevParent.mother === nextParent.mother 
    && prevProps.name === nextProps.name
  )
});

class Page extends PureComponent {
  constructor(props) {
    super(props)
    this.state = {
      name: "unnamed",
      age: 42,
      parent: {
        father: "-",
        mother: "Elissa"
      }
    }
    this.setUserName = this.setUserName.bind(this)
    this.setParentName = this.setParentName.bind(this)
  }

  setUserName() {
    this.setState({...this.state, name: "Mr X" })
  }
  setParentName(){
    this.setState({...this.state, parent: {...this.state.parent, father: "Georg"} })
  }
  shouldComponentUpdate(nextProps, nextState) {
    const {name: cName, age: cAge, parent: cParent} = this.state
    const {name: nName, age: nAge, parent: nParent} = nextState
    const nameHasChanged = (cName !== nName)
    const ageHasChanged = (cAge !== nAge)
    const parentHasChanged = (cParent.father!==nParent.father || cParent.mother!==nParent.mother)
    return (nameHasChanged || ageHasChanged || parentHasChanged);
  }
  

render() {
  console.log("render Page");
  const {name, age, parent} = this.state
  return (
    <Fragment>
      <button onClick={this.setUserName}>
        Set new name "Jack"
      </button>
      <button onClick={this.setParentName}>
        Set new Father "Georg"
      </button>
      <WelcomeMessage 
        name={name} 
        parent={parent} 
      />

    </Fragment>
  );
}
 
};

export default Page;
```



![Screenshot 2020-09-11 at 15 20 31](https://user-images.githubusercontent.com/7538862/92930457-618ef880-f442-11ea-8286-84afa18d2e77.png)


## Solution with flattened state

```jsx
import React, { useState, Fragment } from "react";
const WelcomeMessage = ({ name, father, mother }) => {
  console.log("render WelcomeMessage");
  return (
    <section>
      <div>Hello {name}</div>
      <div>Your Father {father}</div>
      <div>Your Mother {mother}</div>
    </section>
  );
};
const App = () => {
  console.log("render App");
  const [name, setName] = useState("unnamed");
  const [age, setAge] = useState(42);
  const [mom, setMom] = useState("Elissa");
  const [dad, setDad] = useState("-");
  return (
    <Fragment>
      <button onClick={() => setName("Mr X")}>Set new name "Mr X"</button>
      <button onClick={() => setDad("Georg")}>Set new Father "Georg"</button>
      <WelcomeMessage name={name} father={dad} mother={mom} />
    </Fragment>
  );
};
export default App;
```

### Profile after 3 clicks of `Set new name "Mr. X"`

<img width="844" alt="Screenshot 2020-10-16 at 10 29 56" src="https://user-images.githubusercontent.com/16235157/96236443-575d9e00-0f9c-11eb-8cf7-2b6b52f4755b.png">


## Solution with immer

If you have to keep the nested state object, [immer](https://immerjs.github.io/immer/docs/introduction) does a good job of optimising rerenders. It is [an external dependency](https://bundlephobia.com/result?p=immer), but it is a general solution to the problem of updating nested state objects.

```jsx
import React, { useState, Fragment } from "react";
import produce from "immer";

const WelcomeMessage = ({ name, parent }) => {
  console.log("render WelcomeMessage");
  return (
    <section>
      <div>Hello {name}</div>
      <div>Your Father {parent.father}</div>
      <div>Your Mother {parent.mother}</div>
    </section>
  );
};
const App = () => {
  console.log("render App");
  const initState = {
    name: "unnamed",
    age: 42,
    parent: {
      father: "-",
      mother: "Elissa",
    },
  };
  const [user, setUser] = useState(initState);
  const { name, parent } = user;
  return (
    <Fragment>
      <button
        onClick={() =>
          setUser(
            produce((user) => {
              user.name = "Mr X";
            })
          )
        }
      >
        Set new name "Mr X"
      </button>
      <button
        onClick={() =>
          setUser(
            produce((user) => {
              user.parent.father = "Georg";
            })
          )
        }
      >
        Set new Father "Georg"
      </button>
      <WelcomeMessage name={name} parent={parent} />
    </Fragment>
  );
};

export default App;
```

### Profile after 3 clicks of `Set new name "Mr. X"`

<img width="846" alt="Screenshot 2020-10-16 at 10 38 42" src="https://user-images.githubusercontent.com/16235157/96236544-7a884d80-0f9c-11eb-92ea-1f7cd5ccdb6d.png">

# Re-Render tips
Double render every time on DEV
React.StrictMode is making double rendering in DEV (that won’t happen in prod), the documentation actually stating that it is intentionally made that way to warn the user from making extensive side effects. - Google more yourself -

# One more time
If you have a functional component with simple string and one button changing that spring using a Hook function, this component has a child component that renders that value passed by props, react may need to re-render the parent (without rendering the child!) one more time even though the state(string) is exactly the same previously. But react won’t ever go deeper.


# Flatten your deep state
Having a flat State always makes it easier to prevent unnecessary rendering by the default React shallow comparison.


```
// Bad - because react will compare object reference, instead of the data change
fortyGuy = useState({
  name: 'forty',
  age: 40,
  parents: {
    mom: 'Fortiette',
    dad: 'big forty'
  }
})


return <ShowPerson person={fortyGuy}>
```

```
// Good - react will be able to detect data change with no extra effort - always to remember react only do shallow comparision
name = useState('forty')
age = useState(40')
mom = useState('Fortiette')
dad = useState('big forty')

return <ShowPerson name={} name={} mom={mom} dad={dad}>
```


# Wrap up

React documentation is aware of the issue that sometimes we should avoid render (reconciliation) here the [link](https://reactjs.org/docs/optimizing-performance.html#avoid-reconciliation).

Most of the time we are fine, only some situations reconciliation will be noticeable as React says:

![Screenshot 2020-09-18 at 14 09 41](https://user-images.githubusercontent.com/7538862/93595769-a07bfb80-f9b8-11ea-9544-79c7e8e77f7f.png)

So in this very specific case, we are **forced** to use _shouldComponentUpdate_ which is at the end converting your Component to Class.

However, React.memo will be in practice the most used helper function for optimisation.
most of the times to prevent child components from re-rendering with same props.

Only in some case like react doc said, if there is a noticeable issue in parent component - the last weapon is to override shouldComponentUpdate, code will unfortunately look fatter eventually.
