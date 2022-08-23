# Hoisting

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

# Variable Definitions (var, let, const)

- `var` declarations are **globally scoped** or **function scoped** while `let` and `const` are **block scoped**.
- `var` variables can be updated and re-declared within its scope; `let` variables can be updated but not re-declared; `const` variables can neither be updated nor re-declared.
- They are all hoisted to the top of their scope. But while `var` variables are initialized with `undefined`, `let` and `const` variables are not initialized. So if you try to access the variables which are initialized with `let` or `const` you will get a `Reference Error`
- While `var` and `let` can be declared without being initialized, `const` must be initialized during declaration.
