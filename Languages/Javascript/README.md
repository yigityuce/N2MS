- [Hoisting](#hoisting)
  - [Variable Hoisting](#variable-hoisting)
  - [`var` Hoisting](#var-hoisting)
  - [`let` and `const` Hoisting](#let-and-const-hoisting)
- [Scope](#scope)
  - [Block Scope](#block-scope)
  - [Function Scope](#function-scope)
  - [Module Scope](#module-scope)
  - [Global Scope](#global-scope)
- [Variable Definitions (var, let, const)](#variable-definitions-var-let-const)
  - [TDZ - Temporal Dead Zone](#tdz---temporal-dead-zone)
- [Closures](#closures)
  - [Lexical Scoping](#lexical-scoping)
  - [Closure](#closure)
- [Strict Mode](#strict-mode)
  - [Invoking strict mode](#invoking-strict-mode)
    - [For Scripts](#for-scripts)
    - [For Functions](#for-functions)
    - [For Modules](#for-modules)
    - [For Classes](#for-classes)
- [Currying](#currying)
- [Modules](#modules)

# Hoisting

- Hoisting is a JavaScript mechanism where **variables**, **function declarations** and **classes** are moved to the top of their scope before code execution.
- Variable and class declarations are also hoisted, so they too can be referenced before they are declared.

## Variable Hoisting

- **JavaScript only hoists declarations, not initializations!**
- This means that initialization doesn't happen until the associated line of code is executed, even if the variable was originally initialized then declared, or declared and initialized in the same line.
- Until that point in the execution is reached the variable has its **default initialization** (`undefined` for a variable declared using `var`, otherwise **uninitialized**).

> **Note**: Conceptually variable hoisting is often presented as the interpreter "splitting variable declaration and initialization, and moving **just** the declarations to the top of the code".

## `var` Hoisting

- The default initialization of the var is undefined.

```ts
console.log(num); // Returns 'undefined' from hoisted var declaration
var num = 6; // Initialization and declaration.
console.log(num); // Returns 6 after the line with initialization is executed.

// after hoisting
var num;
console.log(num); // Returns 'undefined'
num = 6;
console.log(num); // Returns 6
```

## `let` and `const` Hoisting

- Variables declared with `let` and `const` are also hoisted but, unlike `var`, **are not initialized** with a default value.
- A `ReferenceError` exception will be thrown if a variable declared with `let` or `const` is read before it is initialized.

```ts
console.log(num); // TDZ, Throws ReferenceError exception as the variable is uninitialized
let num = 6; // Initialization

// after hoisting
let num;
console.log(num); // Throws ReferenceError
num = 6;
```

# Scope

- The scope is the current context of execution in which values and expressions are "**visible**" or can be referenced.
- If a variable or expression is not in the current scope, it will not be available for use.
- Scopes can also be layered in a hierarchy, so that **child scopes have access to parent scopes, but not vice versa**.
- JavaScript has the following kinds of scopes:
  - **Global scope:** The default scope for all code running in script mode
  - **Module scope:** The scope for code running in module mode
  - **Function scope:** The scope created with a function
  - **Block scope:** The scope created with a pair of curly braces (a block). variables declared with let or const can belong to this scope.

## Block Scope

- A code block in JavaScript defines a scope for variables declared using `let` and `const`.
- The code block of `if`, `for`, `while` statements also create a scope.

```ts
if (true) {
  // "if" block scope
  const message = "Hello";
  console.log(message); // 'Hello'
}
console.log(message); // throws ReferenceError
```

- `var` is **not** block scoped
- As seen in the above, the code block creates a scope for variables declared using `const` and `let`, however, that's not the case of variables declared using `var`.

> **A code block does not create a scope for var variables, but a function body does.**

## Function Scope

- A function in JavaScript defines a scope for variables declared using `var`, `let` and `const`.

```ts
function run() {
  // "run" function scope
  var message = "Run, Forrest, Run!";
  console.log(message); // 'Run, Forrest, Run!'
}
run();
console.log(message); // throws ReferenceError
```

- `run()` function body creates a scope. The variable `message` is accessible inside of the function scope, but **inaccessible** outside.

## Module Scope

- ES2015 module also creates a scope for variables, functions, classes.
- The variable which is defined in the module is not accessible outside of the module (unless explicitly exported using `export`).
- The module scope makes the module **encapsulated**.
- Every private variable (that's not exported) remains an internal detail of the module, and the module scope protects these variables from being accessed outside.
- The scope is an encapsulation mechanism for code blocks, functions, and modules.

```ts
// FILE: circle.ts
const pi = 3.14159;
console.log(pi); // 3.14159
```

```ts
// FILE: index.ts
import "./circle";
console.log(pi); // throws ReferenceError
```

## Global Scope

- The global scope is the outermost scope.
- It is accessible from any inner (aka local) scope.
- A variable declared inside the global scope is named **global variable**.
- Global variables are accessible from any scope.

# Variable Definitions (var, let, const)

| Keyword | Scope          | Hoisting | Reassignable | Redeclarable |
| ------- | -------------- | -------- | ------------ | ------------ |
| `var`   | Function Scope | Yes      | Yes          | Yes          |
| `let`   | Block Scope    | No       | Yes          | No           |
| `const` | Block Scope    | No       | No           | No           |

- Scope:
  - `var` declarations are **globally scoped** or **function scoped**
    - The scope is global when a var variable is declared **outside** a function.
    - This means that any variable that is declared with `var` outside a function block is available for use in the **whole window**.
    - `var` is function scoped when it is declared within a function.
    - This means that it is available and can be accessed **only within that function**.
  - `let` and `const` are **block scoped**
    - A block is a chunk of code bounded by `{}`.
    - A block lives in curly braces.
    - Anything within curly braces is a block.
    - So a variable declared in a block with `let` or `const` is **only** available for use within that block.
- Re-assignment and re-declaration:
  - `var` variables can be updated and re-declared within its scope;
  - `let` variables can be updated but not re-declared;
  - `const` variables can neither be updated nor re-declared so every `const` variable must be initialized during declaration.
- Hoisting:
  - They are all hoisted to the top of their scope.
  - But while `var` variables are initialized with `undefined`,
  - `let` and `const` variables are not initialized.
  - So if you try to access the variables which are initialized with `let` or `const` you will get a `Reference Error`

## TDZ - Temporal Dead Zone

- TDZ is the term to describe the state where variables are **un-reachable**.
- They are **in scope**, but they **aren't declared**.
- The `let` and `const` variables are in the TDZ from the start of their enclosing scope **until they are declared.**

```ts
{
  // This is the temporal dead zone for the age variable!
  // This is the temporal dead zone for the age variable!
  // This is the temporal dead zone for the age variable!
  // This is the temporal dead zone for the age variable!
  let age = 25; // Whew, we got there! No more TDZ
  console.log(age);
}
```

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

## Invoking strict mode

- Strict mode applies to entire **scripts** or to individual **functions**.
- It **doesn't apply** to block statements enclosed in {} braces; attempting to apply it to such contexts does nothing.

### For Scripts

```ts
// Whole-script strict mode syntax
"use strict";
const v = "Hi! I'm a strict mode script!";
```

### For Functions

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

### For Modules

- ECMAScript 2015 introduced JavaScript modules and therefore a 3rd way to enter strict mode. - The entire contents of JavaScript modules are **automatically in strict mode**, with no statement needed to initiate it.

```ts
function myStrictFunction() {
  // because this is a module, I'm strict by default
}
export default myStrictFunction;
```

### For Classes

- All parts of ECMAScript **classes are strict mode code**, including both class declarations and class expressions — and so also including all parts of class bodies.

# Currying

# Modules
