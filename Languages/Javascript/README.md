# Hoisting

Hoisting is a JavaScript mechanism where variables and function declarations are moved to the top of their scope before code execution.

# Variable Definitions (var, let, const)

- `var` declarations are **globally scoped** or **function scoped** while `let` and `const` are **block scoped**.
- `var` variables can be updated and re-declared within its scope; `let` variables can be updated but not re-declared; `const` variables can neither be updated nor re-declared.
- They are all hoisted to the top of their scope. But while `var` variables are initialized with `undefined`, `let` and `const` variables are not initialized. So if you try to access the variables which are initialized with `let` or `const` you will get a `Reference Error`
- While `var` and `let` can be declared without being initialized, `const` must be initialized during declaration.

# Closures

- A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**).
- In other words, a closure gives you access to an outer function's scope from an inner function.
- In JavaScript, closures are created every time a function is created, at function creation time.

## Lexical Scoping

```ts
function init() {
  var name = "Yigit"; // name is a local variable created by init
  function displayName() {
    // displayName() is the inner function, a closure
    console.log(name); // use variable declared in the parent function
  }
  displayName();
}
init();
```

- `init()` creates a local variable called name and a function called `displayName()`.
- The `displayName()` function is an **inner function** that is defined inside init() and is available only within the body of the init() function.
- Note that the `displayName()` function has no local variables of its own.
- However, since inner functions have access to the variables of **outer functions**, `displayName()` can access the variable name declared in the parent function, `init()`

## Closure

```ts
function makeFunc() {
  const name = "Yigit";
  function displayName() {
    console.log(name);
  }
  return displayName;
}

const myFunc = makeFunc();
myFunc();
```

- Running this code has exactly the same effect as the previous example of the `init()` function above.
- What's different (and interesting) is that the `displayName()` inner function is returned from the outer function before being executed.
- In some programming languages, the local variables within a function exist for just the duration of that function's execution. Once `makeFunc()` finishes executing, you might expect that the name variable would no longer be accessible.
- The reason is that functions in JavaScript form **closures**.
- A closure is the combination of a function and the lexical environment within which that function was declared.
- This environment consists of any local variables that were in-scope at the time the closure was created.
- In this case, `myFunc` is a **reference** to the instance of the function `displayName` that is created when `makeFunc` is run.
- The instance of `displayName` maintains a reference to its lexical environment, within which the variable name exists.
- For this reason, when `myFunc` is invoked, the variable name remains available for use.

# Strict Mode

- JavaScript's strict mode is a way to **opt in** to a restricted variant of JavaScript, thereby implicitly **opting-out** of "**sloppy mode**" (normal mode).
- Strict mode isn't just a subset: it intentionally has different semantics from normal code.
- Strict mode code and non-strict mode code **can coexist**, so scripts can opt into strict mode incrementally.
- Strict mode makes several changes to normal JavaScript semantics:
  - Eliminates some JavaScript silent errors by changing them to throw **errors**.
  - Fixes mistakes that make it difficult for JavaScript engines to perform optimizations: strict mode code can sometimes be made to run faster than identical code that's not strict mode.
  - Prohibits some syntax likely to be defined in future versions of ECMAScript

## Invoking strict mode:

- Strict mode applies to entire **scripts** or to individual **functions**.
- It **doesn't apply** to block statements enclosed in {} braces; attempting to apply it to such contexts does nothing.

### For Scripts:

```ts
// Whole-script strict mode syntax
"use strict";
const v = "Hi! I'm a strict mode script!";
```

### For Functions:

```ts
function myStrictFunction() {
  // Function-level strict mode syntax
  "use strict";
  function nested() {
    return "And so am I!";
  }
  return `Hi! I'm a strict mode function! ${nested()}`;
}

function myNotStrictFunction() {
  return "I'm not strict.";
}
```

### For Modules:

- ECMAScript 2015 introduced JavaScript modules and therefore a 3rd way to enter strict mode. - The entire contents of JavaScript modules are **automatically in strict mode**, with no statement needed to initiate it.

```ts
function myStrictFunction() {
  // because this is a module, I'm strict by default
}
export default myStrictFunction;
```

### For Classes:

- All parts of ECMAScript **classes are strict mode code**, including both class declarations and class expressions — and so also including all parts of class bodies.
