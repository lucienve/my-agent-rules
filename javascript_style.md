---
trigger: "*.js"
description: JavaScript coding standards, testing, and stylistic requirements.
---

# JavaScript Directives

You must strictly adhere to these coding standards at all times to ensure
consistent JavaScript code quality.

## JavaScript Formatting & Style

- **Syntax / Style:** Strictly follow the
  [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html).
  Use modern ES6+ features wherever appropriate.
- **Linting:** Automatically invoke `eslint` against any modified JavaScript
  files, using the Google ESLint configuration (`eslint-config-google`).
  Strictly resolve any lint warnings, unused imports, or formatting issues
  before returning the code.
- **Documentation:** Use JSDoc strings for all modules, classes, and exposed
  functions.
- **Testing:** Use `jest` for testing. Rigorously test core logic, edge cases,
  and asynchronous operations.
- **Dependencies:** Manage packages and dependencies using `npm`. Always
  maintain an up-to-date `package.json` and `package-lock.json`.
