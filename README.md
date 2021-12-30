# Front End Standards & Best Practices

Hello, front end engineer!

The main goal of this repo is to centralize all the relevant information for front end engineers, from style guide to small tips. Feel free to contribute if you think something else can be added here!

## Communication

- Email: Frontend-chapter@talview.com
- Slack channel: **#team-frontend**

## Front End Chapter

Once a week we have a meeting to discuss front end topics with all front end engineers from talview, we usually have a clear agenda of things we want to improve or talk about it. You can find the meetings notes [here](https://docs.google.com/document/d/18KRxPasDyN7Ocf7FOKfQsgj_G8Tv2lnP4fIBxmtRZjo/edit).

## Standard Tech Stack

- [x] React
- [x] Typescript
- [x] Jest
- [x] Material UI
- [x] React Testing Library
- [x] Cypress

## Design System

We have a well established design system, it is called [talstrap](https://github.com/talview/talstrap), which is built on top of the Material UI Library. 
Please take a look at the code [here](https://github.com/talview/talstrap). 
You can also see the set of components through [Storybook](https://s3-eu-west-1.amazonaws.com/talstrap.io/latest/index.html?path=/story/button--examples)
The Design inspiration comes from the set of designs that can be found [here](https://gallery.io/projects/MCHbtQVoQ2HCZRG8McrLXvBI/files/MCGUYFPVVRZ-Z5ZXcTw-N57h). Please feel free to contribute to [talstrap](https://github.com/talview/talstrap) if any design elements are missing. 

## Development Workflow

### Issues creation

When starting a new conversation for improvement or bug fixing, we should always start by creating issue on frontend-standards.
To make easier to track Issues we agreed to prefix them with priorities as described below:

- **P0:** for high priorities (security concern)
- **P1:** for medium (style guides, paradigms)
- **P2:** for nice to have (everything else)

We can optionally add some themes with priorities as well, here some examples:

- [**P0/RFC**] your title here

- [**P0/Security**] your title here

- [**P1/paradigm**] your title here

We should add the issue on the project [Frontend Chapter Issues](https://github.com/talview/frontend-standards/projects/1), in the `TODO` column which will be added to `Under Discussion` column during the frontend chapter meeting.

#### Linking Issues

If you are working on some Frontend Chapter Issue please also create a JIRA ticket for your team's board and provide a link for it in the issue - this way we could help you to prioritize the Frontend Chapter work within your team.

When creating a JIRA ticket make sure you add a link to the issue created on frontend-standards.

## Front End Style Guide

## Front End Best Practices

- In case code is shared between multiple applications and not part of the Design System, a discussion has to be initiated in the Front End Chapter for the need of creating libraries/Micro Frontends for the respective bit of code, which has to be approved before moving forward

- Use destructured named imports for React instead of default imports or namespace imports.

```tsx
// Good
import { FC, useState } from 'react'

// Avoid
import React from 'react'

// Avoid
import * as React from 'react'
```

- Use [Design System](https://github.com/talview/talstrap/) components and re-imports if possible. Refrain from using old [talstrap](https://github.com/talview/talstrap-deprecated/) and direct `material-ui`.

```tsx
// Good
import { TextField, makeStyles, Theme } from '@talview/talstrap'
import { Done } from '@talview/talstrap-icons'

// Avoid
import { TextField, makeStyles, Theme } from '@talview/talstrap'
import { Done } from '@talview/talstrap/dist/icons'

import { TextField, makeStyles, Theme } from '@material-ui/core'
import { Done } from '@material-ui/icons'
```

- Store all of your styles in JSS.
  - Use themes for [colors](https://github.com/talview/talstrap/blob/master/packages/talstrap/src/styles/options/palette.ts) and spaces.
  - Use [Typography](https://github.com/talview/talstrap/blob/master/packages/talstrap/src/styles/options/typography.ts#L16) for texts
  - Use hooks to consume styles.

```tsx
// Good
const useStyles = makeStyles<Theme>((theme) => ({
  title: {
    marginRight: theme.spacing(4),
    fontWeight: theme.typography.fontWeightBold,
    borderBottom: `1px solid ${theme.palette.divider}`,
  },
}))

export const MyComponent: FC<MyComponentProps> = (props) => {
  const classes = useStyles()
  return <div className={classes.title} />
}
```

- Use only Types for component properties (no Interface), because you might [merge the props unintentionally](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#merging-interfaces)

```tsx
// Avoid
export const MyComponent: FC<{ prop1: string; prop2: number[] }>

// Avoid
export interface MyComponentProps {
  prop1: string
  prop2: number[]
}
export const MyComponent: FC<MyComponentProps>

// Good
export type MyComponentProps = {
  prop1: string
  prop2: number[]
}
export const MyComponent: FC<MyComponentProps>
```

- Prefer to use functional components instead of classes.

```tsx
// Good
export const MyComponent: FC<MyComponentProps> = (props) => {
  return <div />
}

// Avoid
export class MyComponent extends Component<MyComponentProps> {
  render() {
    return <div />
  }
}
```

- Use `FC` type for your functional components.

```tsx
// Good
export const MyComponent: FC<MyComponentProps> = (props) => {
  return <div />
}

// Avoid
export const MyComponent = (props: MyComponentProps) => {
  return <div />
}
```

- Keep local state in `useState` hook. (TODO: Define solution for global state).

```tsx
// Good
export const MyComponent: FC<MyComponentProps> = ({ initialState }) => {
  const [localState, setLocalState] = useState(initialState)
}
```

- Use `translate` function from `useI18n` hook for i18n.

```tsx
// Good
export const MyComponent: FC<MyComponentProps> = (props) => {
  const { translate } = useI18n()
}
```

- Keep your components small, split bigger components into several small ones.
- Avoid having a lot of complex logic in one place. Try to split your logic into simple parts and assign these parts to variables / functions.
- Use types for all variables / functions that are not typed automatically, avoid to have implicit or explicit 'any' type.
- Don't put data fetching / transforming logic inside components, create separated services for this.
- If you need to extend your AppStore, create separate store instead.
- Always write unit tests for new logic.
- Add all your files to `tsconfig.strict.json` file.
- Fix ESLint issues if present, don't ignore them.
- Fix CodeClimate issues if detected, don't ignore them.
- If some parts of the code don't follow these practices, consider to refactor them on touch.

## State Management

### Current state

Historically, we are using mobx-state-tree as a state-management.
Also some legacy code is using mobx as state management
New features are built with only react features (hooks, context).

### Target state

- For major models that are shared between different pages/tabs (e.g: shipment, tp, health ..), we don't have a plan to replace them by an other state management yet.
- All mobx local states have to been replaced by native react state
- Small mst models should be replaced by native react state
- New features have to been created with native react features.
  If you feel the need of a state management for your next features, please free free to share your needs, we will be happy to find a solution.
- Replace usage of `mobx-react` by `mobx-react-lite`

## [React performance and profiling](/guides/react-profiler-and-performance.md)

## Userlane dev notes

You might be asked to work with Userlane or you want to integrate it to a microfrontend project, please use these notes to help you out:

- Both projects almost share the same config/files - only minor changes

- Development/Sandbox/Production kubernetes config are different so watchout for passed ENV variable, most likely the solution will be just copying passed ENV from 1 ENV to another ( do not get dragged into code only, ENV files in pods also matter )

## Release Notes

Release Notes are required whenever something is released in the Frontend. This is especially vital in [talstrap](https://github.com/talview/talstrap) since all the products have to update it. Following is the standard by which release notes are to be distributed:

### vX.X has been released! [Optional]

**What's new?**

- New 1
- New 2

**Bugs and Improvements**

- Bug 1
- Bug 2
- Improvement 1

**Recommendatations(if any)**

- Recommendation 1
- Recommendation 2

### Workflow for breaking changes in shared packages

Before releasing breaking changes in shared packages such as [talstrap](https://github.com/talview/talstrap), other teams must be aware of these changes so that they could adapt/object to them accordingly. Following are some points that need to be kept in mind before a breaking change PR is merged:

- At least one person from each team should accept the PR.
- The author of the PR should wait for at least half a day(4 hrs) before he/she reaches out to other teams if no review has been received.
- In case of urgency or unavailability, the author could escalate this to the team leads.
