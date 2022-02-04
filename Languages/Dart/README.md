- [Introduction](#introduction)
- [Important Concepts](#important-concepts)
- [Variables](#variables)
  - [Default Value](#default-value)
  - [Late Variables](#late-variables)
  - [Final and const](#final-and-const)
- [Built-in Types](#built-in-types)
  - [Numbers](#numbers)
  - [Strings](#strings)
  - [Booleans](#booleans)
  - [Lists](#lists)
  - [Sets](#sets)
  - [Maps](#maps)

# Introduction

- Document created for Dart version 2.15
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

**Everything is object**

Everything you can place in a variable is an **object**, and every object is an instance of a class. Even **numbers**, **functions**, and **null** are objects. All objects inherit from the `Object` class.

**Type Infer**

Although Dart is strongly typed, **type annotations are optional** because Dart can infer types. In the code above, `number` is inferred to be of type `int`.

**Nullable**

Variables can’t contain `null` unless you say they can. You can make a variable nullable by putting a question mark (`?`) at the end of its type. For example, a variable of type `int?` might be an integer, or it might be null.

**Any Type**

When you want to explicitly say that any type is allowed, use the type `Object?`, `Object`, or — if you must defer type checking until runtime — the special type `dynamic`.

**Generics**

Dart supports generic types, like `List<int>` (a list of integers) or `List<Object>` (a list of objects of any type).

**Top Level Functions - Local Functions**

Dart supports top-level functions (such as `main()`), as well as functions tied to a class or object (static and instance methods, respectively). You can also create functions within functions (**nested or local functions**).

**Access Modifiers**

Dart doesn’t have the keywords `public`, `protected`, and `private`. If an identifier starts with an underscore (`_`), it’s private to its library.

# Variables

```dart
var name = 'Yigit'; // type infer
Object name = 'Yigit'; // any type
String name = 'Yigit'; // explicit type
```

## Default Value

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

## Late Variables

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

## Final and const

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

# Built-in Types

- Every variable in Dart refers to an object—an instance of a class—you can usually use constructors to initialize variables.
- Some of the built-in types have their own constructors.
  - For example, you can use the `Map()` constructor to create a map.
- Some other types also have special roles in the Dart language:
  - `Object`: The superclass of all Dart classes except `Null`.
  - `Future` and `Stream`: Used in asynchrony support.
  - `Iterable`: Used in for-in loops and in synchronous generator functions.
  - `Never`: Indicates that an expression can never successfully finish evaluating. Most often used for functions that always throw an exception.
  - `dynamic`: Indicates that you want to disable static checking. Usually you should use `Object` or `Object?` instead.
  - `void`: Indicates that a value is never used. Often used as a return type.

## Numbers

- `int`
  - Integer values no larger than 64 bits
- `double`
  - 64-bit (double-precision) floating-point numbers
- `num`
  - variable can have both `int` and `double` values

```dart
// integer type
var x = 1;
var hex = 0xDEADBEEF;
var exponent = 8e5;

// double type
var y = 1.1;
var exponents = 1.42e5;

// num type
num z = 1; // z can have both int and double values
z += 2.5;
```

- Type casting from and to `String`:

```dart
var one = int.parse('1'); // String -> int
var onePointOne = double.parse('1.1'); // String -> double

String oneAsString = 1.toString(); // int -> String
String piAsString = 3.14159.toStringAsFixed(2); // double -> String
```

- Literal numbers are **compile-time constants**.
- Many arithmetic expressions are also compile-time constants, as long as their operands are compile-time constants.

```dart
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
```

## Strings

- A Dart string (`String` object) holds a sequence of **UTF-16** code units.
- You can use either single or double quotes to create a string

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

- You can put the value of an expression inside a string by using `${expression}`.
- If the expression is an identifier, you can skip the `{}`.
- To get the string corresponding to an object, Dart calls the object’s `toString()` method

```dart
var s = 'string interpolation';

assert('Dart has $s, which is very handy.' == 'Dart has string interpolation, which is very handy.');
assert('That deserves all caps. ${s.toUpperCase()} is very handy!' == 'That deserves all caps. STRING INTERPOLATION is very handy!');
```

- You can concatenate strings using adjacent string literals or the `+` operator:

```dart
var s1 = 'String '
    'concatenation'
    " works even over line breaks.";
assert(s1 ==
    'String concatenation works even over '
        'line breaks.');

var s2 = 'The + operator ' + 'works, as well.';
assert(s2 == 'The + operator works, as well.');
```

- Another way to create a multi-line string: use a **triple quote** with either single or double quotation marks:

```dart
var s1 = '''
You can create
multi-line strings like this one.
''';

var s2 = """This is also a
multi-line string.""";
```

- You can create a **“raw”** string by prefixing it with `r`:

```dart
var s = r'In a raw string, not even \n gets special treatment.';
```

## Booleans

- To represent boolean values, Dart has a type named `bool`.
- Only two objects have type bool: the boolean literals `true` and `false`, which are both compile-time constants.
- You can’t use code like `if (nonbooleanValue)` or `assert (nonbooleanValue)`. Instead, explicitly check for values, like this:

```dart
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints == 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```

## Lists

- The most common collection in nearly every programming language is the array, or ordered group of objects.
- In Dart, arrays are `List` objects

```dart
var list = [1, 2, 3];

// The trailing comma doesn’t affect the collection.
var list2 = [
  'Car',
  'Boat',
  'Plane',
];
```

- Lists use **zero-based indexing**, where:
  - `0` is the index of the first value
  - `list.length - 1` is the index of the last value
- You can get a list’s length and refer to list values just as you would in JavaScript:

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);
```

- To create a list that’s a compile-time constant, add `const` before the list literal:

```dart
var constantList = const [1, 2, 3];
// constantList[1] = 1; // This line will cause an error.
```

- Dart 2.3 introduced the **spread operator** (`...`) and the **null-aware spread operator** (`...?`)

```dart
var list = [1, 2, 3];
var list2 = [0, ...list];
assert(list2.length == 4);
```

```dart
var list;
var list2 = [0, ...?list];
assert(list2.length == 1);
```

- Dart also offers **collection if** and **collection for**, which you can use to build collections using conditionals (`if`) and repetition (`for`)

```dart
var nav = [
  'Home',
  'Furniture',
  'Plants',
  if (promoActive) 'Outlet'
];
```

```dart
var listOfInts = [1, 2, 3];
var listOfStrings = [
  '#0',
  for (var i in listOfInts) '#$i'
];
assert(listOfStrings[1] == '#1');
```

## Sets

- A set in Dart is an unordered collection of **unique** items.
- Dart support for sets is provided by set literals and the `Set` type.

```dart
var fruits = {'apple', 'orange', 'blueberry', 'raspberry'};
```

- Dart infers that `fruits` has the type `Set<String>`. If you try to add the wrong type of value to the set, the analyzer or runtime **raises an error**.
- To create an **empty set**
  - use `{}` preceded by a **type argument** or
  - assign `{}` to a variable of **type Set**:

```dart
var names = <String>{};
// Set<String> names = {}; // This works, too.
// var names = {}; // Creates a map, not a set.
```

- Add items to an existing set using the `add()` or `addAll()` methods.
- Use `.length` to get the number of items in the set
- Sets support spread operators (`...` and `...?`) and **collection if** and **collection for**, just like lists do.

```dart
var tropicalFruits = <String>{
  'kiwi',
  'coconut',
  'mango'
};
var allFruits = <String>{
  'banana',
  ...tropicalFruits,
};

allFruits.add('strawberry');
allFruits.addAll(fruits);
assert(allFruits.length == 9);
```

## Maps

- Map is an object that associates keys and values.
- Both keys and values can be **any type of object**.
- Each key occurs **only once**, but you can use the same value multiple times.
- Maps support spread operators (`...` and `...?`) and **collection if** and **collection for**, just like lists do.

```dart
// Dart infers the type Map<String, String>
var gifts = {
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

// Dart infers the type Map<int, String>
var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

var days = Map<int, String>();
days[1] = 'Monday';
days[3] = 'Wednesday';
days[5] = 'Friday';
```

- Retrieve a value from a map the same way you would in JavaScript
- If you look for a key that isn’t in a map, you get a null in return
- Use .length to get the number of key-value pairs in the map

```dart
assert(days[1] == 'Monday');
assert(days[2] == null);
assert(days.length == 3);
```
