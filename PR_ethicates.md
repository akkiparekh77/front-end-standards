# Summary

Guidelines for effective and constructive Pull Request [PR] reviews.

# Motivation

We want highly effective review cycles. The reviewing and implementing party should have the same expectations towards the goal of the code review and requirements. That's why we want to establish a common understanding what reviews are about. 

The approaches for reviewing a technical debt / refactoring PR, a crew internal feature review and a cross-crew review might differ. We will discuss in more detail how different kinds of reviews shall be conducted. 

# Detailed design

## 1. General

### 1.1 What we care about

1. Logic
2. Readability
3. Quality of code (tests, low complexity, easily extendable design)
4. Getting PRs merged quickly
5. Naming (meaning and grammar)
6. Business impact
7. Design coherence with our standards
8. Developer Experience


### 1.2 What we DO

1. We maintain the code-style inside our cores. We respect the fact that other cores might have a different code-style.
2. We create small PRs (<250 lines of code excluding auto-generated files).
3. We focus on business value.
4. We leave constructive criticism, which stays in the scope of the current PR and problem it’s solving.
5. We treat people with respect and write comments in a respectful and educational way.
6. We care as much as the implementer about finding productive solutions to move a PR to a completed state.
7. We try to improve the code we are working on, on every touch, and leave it cleaner than it was before. (*-Scout rule)
 

### 1.3 Criteria of constructive feedback

1. In case of incorrect logic: It gives example, why this solution is not correct and suggests how to change it.
2. In case of incorrect naming: Explain why this name is ambiguous or doesn’t reflect the meaning of the variable/class/interface. Suggest a better name, or at least brings this issue to attention.

### 1.4 Types of PR Reviews

#### 1.4.1 Feature PR
**Things that SHOULD be fixed before approval**
- logical errors
- readability and complexity concerns
- introduction of new technical debt (without prior agreement)
- current code guideline violations (of respective core or SDK), especially if you are the maintainer
- code coverage (if lacking a test plan, if lacking proof that system works)



**Things that SHOULD NOT be demanded to fix before approval**
- issues discovered that are old technical debt (that take considerable amount of work to fix) --> Instead write them into technical debt list and evaluate risk, complexity and prio. (Follow up on this issue and address it in your team to fit it in your workflows to fix tech debt)
- test coverage of features that were not touched --> same as above
- architectural changes --> optimally the architecture should be agreed upon BEFORE starting to work on the feature

**Things that need to be discussed on case by case basis** 
When you have an opinionated topic where it seems it's just your word against another, get a neutral third person on board and quickly discuss a solution. Always try to find a common ground or a compromise where both parties agree in the end. It's not about winning or losing, it's about the most productive way to move forward.

#### 1.4.2 Tech Debt / Refactor PR

**Things that SHOULD be fixed before approval**
- logical errors
- readability and complexity concerns
- introduction of new technical debt
- current code guideline violations (of respective core or SDK), especially if you are the maintainer
- suggestions about improving architecture (related to the problem the PR is trying to solve)

#### 1.4.3 Proof of Concept [PoC] PR's 

When you're implementing a PoC of some idea you had, it's worthwhile to really sit together with someone, or multiple people and discuss the solution beforehand. 
Announce it in the #engineering channel and write a mail to engineering@talview.com to notify interested people.
Just creating a PR and not talking to anyone might lead to lots of side-tracked discussions and delays of putting it into production.

## 2. How to avoid PRs with too many comments

### 2.1 Tip 1 - Prior architectural agreement with future reviewer
When you are starting working on a bigger topic involving architectural changes sit together with another person who is experienced in the code you are touching to discuss a proposed solution. Before, make sure to agree on product, operational and architectural requirements with stakeholders. These people should be the ones reviewing your PR later. When you agree on a solution it is easier for the reviewer to comprehend your changes. The PR will be reviewed faster, and the comments will mostly not be about architecture. So you won’t have to do your implementation twice.

### 2.2 Tip 2 - Talk to the implementer when you see lots of issues
As a reviewer, if I am reviewing a PR and find lots of issues (> 10 comments) with it, then it is better to sit down with the person and quickly talk it over. Especially if you are doing a review for the first time for this person. Just leaving a PR with 50 comments and then not even talking to the person might be considered rude, and people might get defensive. So go out there and add some human interaction to your daily routine. By talking it over with the person conflicts will be prevented, discussions can be fruitful and agreements can be reached faster.

As the implementer, when you see you got 50 comments on your PR, do not dispair. Keep in mind this means, somebody read your code very thoroughly and generally found areas to ask questions about, applaud or to improve. It does not necessarily mean your code is bad. Maybe it even inspired the reviewer to come up with even better ideas and solutions. So usually it's a chance for both of you to grow. If there are some parts where you don't agree, you can still discuss them, and maybe sometimes get even a third person on board and an even better result. And don't hesitate to use one of the whiteboards to explain your solutions. It helps to clear things up a lot!

## 3. Deep Dive

More detailed examples and explanations of previous topics. Why we do, the things we do.

### On 1.2.1  We maintain the code-style inside our cores and respect the fact that other cores might have a different code-style.
If you notice a vastly different code-style in another repository try to understand the reasoning behind it. Maybe you can learn a thing or two from the solutions they came up with. Maybe you can spot problems and suggest solutions if you see any problems. But if the goal of the PR is a different one, it's not the time to discuss improvement of major parts of the code-style. Rather keep it consistent!

### On 1.2.2 We create small PRs (< 250 lines)
Small PRs are easier and faster to review. It improves the general code quality, bugs are not missed so easily. So we favor small incremental PRs over huge PRs that are all encompassing. It is not always possible. But if you search deep within your heart, you will find a way to do it! 

### On 1.2.4 We leave constructive criticism, which stays in the scope of the current PR and problem it’s solving.

We focus on the scope of the current PR and its problem solving, means that we don’t force the implementer to fix other unrelated issues while we are reviewing this PR. We might make a comment if we discover something considered Tech Debt and add it to the corresponding crews’ Tech Debt List.

Especially when it comes to Feature PRs it might be extremely unproductive and distracting for the goal of the PR to add lots of unrelated Tech Debt comments. The implementer will not be able to focus on the things he needs to fix to get the PR approved, but get distracted.

For PR’s that are specifically aimed at fixing some tech debt, it might be an appropriate thing to do.
So always ask yourself before commenting: 

- Am I contributing to the goal of getting the problem solved this PR wants to solve? 
- Is the issue I’m addressing critical to get this thing done or might it be deferred to a later point?
- Am I introducing new technical debt with this PR that wasn’t there prior and will really hurt us in the long run?

The GOAL should be to leave as as many TODO comments as needed, to get the PR to an acceptable state. While you are adding the comments you can also be educational. But it should NEVER be desired to add the most comments you possibly can, just because you can. So just remember when you leave a comment, there is always another human being on the other side. We are equals that work on the same goal to get things done in a maintainable and productive way!

# Drawbacks

- Restricting the PR review process might lead to people restraining from criticism where it is valid.

# Alternatives

No alternatives considered

# Adoption strategy

Everyone should start taking these guidelines into consideration in their reviews. If you notice someone giving you a review not conforming to them, send him a link to this RFC. This RFC can be modified and adopted as we continue.

# How we teach this

- Send this around in a group mail and announce in Slack channel.
- Link in PR templates
- Code of conduct
- Add link to Contribution guidelines
