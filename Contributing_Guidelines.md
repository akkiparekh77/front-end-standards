# Contributing to Talview Frontend Applications

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting a bug
- Discussing the current state of the code
- Submitting a fix
- Proposing new features
- Becoming a maintainer

## We Develop with Github, So All Code Changes Happen Through Pull Requests

We use github to host code, to track issues and feature requests, as well as accept pull requests. Pull requests are the best way to propose changes to the codebase. We actively welcome your pull requests:

1. Clone the repo and create your branch from `main`.
2. If you've added code that should be tested, add tests, if possible.
3. If you've changed APIs, update the documentation, if possible.
4. Make sure your code passes all CI Checks.
5. Issue that pull request!

## Use a Consistent Coding Style

- If possible, introduce the smallest possible logical change - the total number of lines added should be around 250, to improve PR reviews
- Do not import REACT in your file(s) unless needed
- For issues with design, consult respective designer and use it from there. Design related code should not be approved in a PR
- Code not related specifically to design like `margin, padding, etc` are allowed to be added to TMS
- Use `theme` for all CSS, except for properties that don't require theme, like `position, width, etc`
- Use `react-testing-library` for writing unit tests
- Whenever an API Model is changed in the Backend, the respective model should be changed in the repository
- Avoid adding code to Mobx/Mobx State Tree since the version is old and we are moving to React Hooks
