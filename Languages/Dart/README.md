# Version

- Document created for Dart version 2.15

# Introduction

- Strongly typed
- Null safe
- Type infer

# Important Concepts

```dart
// Define a function.
void printInteger(int aNumber) {
  print('The number is $aNumber.'); // Print to console.
}

// This is where the app starts executing.
void main() {
  var number = 42; // Declare and initialize a variable.
  printInteger(number); // Call a function.
}
```

#### **Everything is object**

Everything you can place in a variable is an **object**, and every object is an instance of a class. Even **numbers**, **functions**, and **null** are objects. All objects inherit from the `Object` class.

#### **Type Infer**

Although Dart is strongly typed, **type annotations are optional** because Dart can infer types. In the code above, `number` is inferred to be of type `int`.

#### **Nullable**

Variables can’t contain `null` unless you say they can. You can make a variable nullable by putting a question mark (`?`) at the end of its type. For example, a variable of type `int?` might be an integer, or it might be null.

#### **Any Type**

When you want to explicitly say that any type is allowed, use the type `Object?`, `Object`, or — if you must defer type checking until runtime — the special type `dynamic`.

#### **Generics**

Dart supports generic types, like `List<int>` (a list of integers) or `List<Object>` (a list of objects of any type).

#### **Top Level Functions - Local Functions**

Dart supports top-level functions (such as `main()`), as well as functions tied to a class or object (static and instance methods, respectively). You can also create functions within functions (**nested or local functions**).

#### **Access Modifiers**

Dart doesn’t have the keywords `public`, `protected`, and `private`. If an identifier starts with an underscore (`_`), it’s private to its library.

## Variables:

```dart
var name = 'Yigit'; // type infer
Object name = 'Yigit'; // any type
String name = 'Yigit'; // explicit type
```

### Default Value:

- Uninitialized variables that have a **nullable** type have an initial value of `null`.
- Even variables with numeric types are initially `null`, because numbers — like everything else in Dart—are **objects**.

```dart
int? lineCount; // default value is null
assert(lineCount == null);
```

- non-nullable variables must be initialized before use them
- You don’t have to initialize a **local variable** where it’s declared, but you do need to assign it a value **before it’s used**
- For example, the following code is valid because Dart can detect that `lineCount` is non-null by the time it’s passed to `print():`

```dart
int lineCount;

if (weLikeToCount) {
  lineCount = countLines();
} else {
  lineCount = 0;
}

print(lineCount);
```

### Late Variables:

- Dart 2.12 added the `late` modifier, which has two use cases:
  - Declaring a non-nullable variable that’s initialized after its declaration
  - Lazily initializing a variable
- Often Dart’s control flow analysis can detect when a non-nullable variable is set to a non-null value before it’s used, but sometimes analysis fails.
- Two common cases are that Dart often can’t determine whether they’re set, so it doesn’t try:
  - top-level variables
  - instance variables
- If you’re sure that a variable is set before it’s used, but Dart disagrees, you can fix the error by marking the variable as late:

```dart
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

- When you mark a variable as `late` but initialize it at its declaration, then the initializer runs the first time the variable is used.
- This lazy initialization is handy in a couple of cases:
  - The variable **might not be needed**, and initializing it is costly.
  - You’re initializing an instance variable, and its **initializer method** needs access to this.
- In the following example, if the temperature variable is never used, then the expensive `_readThermometer()` function is never called:

```dart
// This is the program's only call to _concatNameAndSurname().
late String username = _concatNameAndSurname(); // Lazily initialized.
```

### Final and const:

- If you **never** intend to change a variable, use `final` or `const`, either instead of `var` or in addition to a type.
- A final variable can be set only once.
- `const` variable is a compile-time constant. (Const variables are implicitly final.)
- `final` vs `const`:
  - `final` object **cannot be modified**, its fields **can be changed**.
  - `const` object and its fields c**annot be changed**: they’re **immutable**.

```dart
final name = 'Yigit'; // Without a type annotation
final String surname = 'Yuce';
```

- Use `const` for variables that you want to be **compile-time constants**.
- If the `const` variable is at the class level, mark it `static const`
- Where you declare the variable, set the value to a compile-time constant such as:
  - number literal
  - string literal
  - const variable
  - result of an arithmetic operation on constant numbers

```dart
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere
```

- The const keyword isn’t just for declaring constant variables.
- You can also use it to create **constant values**, as well as to declare constructors that create constant values.
- Any variable can have a constant value.
- You can change the value of a non-final, non-const variable, even if it used to have a const value.

```dart
var foo = const [];
final bar = const [];
const baz = []; // Equivalent to `const []`

foo = [1, 2, 3]; // Was const []
baz = [42]; // Error: Constant variables can't be assigned a value.
```
