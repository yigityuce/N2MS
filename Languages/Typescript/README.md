- [General](#general)
  - [Working with Other JavaScript Libraries](#working-with-other-javascript-libraries)
  - [Links](#links)
- [Types](#types)
  - [string](#string)
  - [number](#number)
  - [array](#array)
  - [tuple](#tuple)
  - [enum](#enum)
  - [any](#any)
  - [void](#void)
  - [undefined & null](#undefined--null)
  - [never](#never)
  - [object](#object)
  - [function](#function)
- [Type Assertion (Type Casting)](#type-assertion-type-casting)
- [Functions](#functions)
  - [Function Type](#function-type)
  - [Optional Parameters](#optional-parameters)
  - [Default Parameters](#default-parameters)
  - [Rest Parameters (spread)](#rest-parameters-spread)
  - [Overloading](#overloading)
- [Interfaces](#interfaces)
  - [Optional Properties](#optional-properties)
  - [Readonly Properties](#readonly-properties)
  - [Excess Property Checks](#excess-property-checks)
  - [Function Types](#function-types)
  - [Hybrid Types](#hybrid-types)
  - [Indexable Types](#indexable-types)
  - [Implementing an Interface](#implementing-an-interface)
  - [Extending Interfaces](#extending-interfaces)
- [Classes](#classes)
  - [Inheritance](#inheritance)
  - [Access Modifiers](#access-modifiers)
  - [Readonly Modifier](#readonly-modifier)
  - [Parameter Properties](#parameter-properties)
  - [Getter & Setter](#getter--setter)
  - [Static Properties](#static-properties)
  - [Abstract](#abstract)
  - [Constructor Signature](#constructor-signature)
- [Generics](#generics)
  - [Generic Types](#generic-types)
    - [Generic Function Type](#generic-function-type)
    - [Generic Interface](#generic-interface)
    - [Generic Class](#generic-class)
    - [Generic Constraint](#generic-constraint)
- [Enum](#enum)
  - [Computed and Constant Members](#computed-and-constant-members)
  - [Enum Member Types](#enum-member-types)
  - [Union Enums](#union-enums)
  - [Reverse Mapping](#reverse-mapping)
  - [Const Enums](#const-enums)
- [Advanced Types](#advanced-types)
  - [Intersection Types](#intersection-types)
  - [Union Types](#union-types)
    - [Discriminated Unions](#discriminated-unions)
    - [Type Guards](#type-guards)
      - [Type Assertion Guard](#type-assertion-guard)
      - [**typeof** Guards](#typeof-guards)
      - [**instanceof** Guards](#instanceof-guards)
  - [Nullable Types](#nullable-types)
  - [Type Alias](#type-alias)
  - [Literal Types](#literal-types)
    - [String Literal Types](#string-literal-types)
    - [Numeric Literal Types](#numeric-literal-types)
  - [Index Types](#index-types)
    - [Index Type Query Operator](#index-type-query-operator)
    - [Indexed Access Operator](#indexed-access-operator)
  - [Mapped Types](#mapped-types)
  - [Conditional Types](#conditional-types)
    - [Predefined Conditional Types](#predefined-conditional-types)
- [Namespace](#namespace)
  - [Multifile Namespaces](#multifile-namespaces)
  - [Ambient Namespaces](#ambient-namespaces)
- [Modules](#modules)
  - [Ambient Modules](#ambient-modules)
- [Decorators](#decorators)
  - [Decorator Factories](#decorator-factories)
  - [Class Decorators](#class-decorators)
  - [Method Decorators](#method-decorators)
  - [Accessor Decorators](#accessor-decorators)


# General
* Notes for v3.4
* File extension is **.ts**
* Plain javascript code can be used in ts file.
* Compile with:

```bash
tsc filename.ts
```

## Working with Other JavaScript Libraries
* To describe the shape of libraries not written in TypeScript, we need to declare the API that the library exposes. 
* Most JavaScript libraries expose only a few top-level objects.
* We call declarations that don’t define an implementation “ambient”. 
* Typically these are defined in .d.ts files. 
* You can think of these as .h files.

## Links
  * [Playground](https://www.typescriptlang.org/play/index.html)

  
# Types
## string
* single quoto
* double quoto
* backtick

```ts
let strVar: string = `2 + 2 = ${ 2 + 2 }`; 
```

## number
* dec
* hex (0x...)
* octal (0o...)
* binary (0b...)

```ts
let hexVar: number = 0xAB12;
```

## array

```ts
let arr: number[] = [1, 2, 3];
let arr2: Array<number> = [1, 2, 3];
```


## tuple
* to annotate array with fixed number of element

```ts
let tpl: [number, string];
tpl = 1; // valid
tpl = 'two'; // valid
tpl = false; // error
```

## enum
* default starts with 0
* can be changed
* enum's number indexed value is the name of the enum:

```ts
enum Color { RED = 5, GREEN, BLUE = 10 }
let clr: Color = Color.RED; // 5
let clrStr: string = Color[10]; // 'BLUE'
```

## any
* any type is valid
* like vanilla javascript

```ts
let myvar: any = 1; // valid
myvar = 'this is string' // valid
myvar = false; // valid

let anyArr: any[] = [1, 'two', false]; // valid
```

## void 
* commonly used at funtion return type declaration
* void typed variable can only be:
  * undefined
  * null

```ts
function print (): void {
    console.log('Print sth.');
}
```

## undefined & null
* not useful
* base type of all other types
  * Ex: you can assign one of them to number or string etc.
  * unless --strictNullChecks flag is set

```ts
let myvar: undefined = undefined;
let myvar2: null = null;
```


## never
* represents the type of values that never occur
* For ex. function expression that always throws an exception or one that never returns

```ts
function error (message: string): never {
    throw new Error(message);
}
```


## object
* represents non-primitive type

```ts
let myObj: object = {
    key1: 'val1',
    key2: 'val2'
};
```

## function
* return type must be always defined even if it is **void**

```ts
let cb: (value: string, index: number) => boolean;

cb = function (v: string, i: number): boolean {
    return (v.length > 0);
}

let strArr: string[] = ['Yigit', '', 'Yuce', ''];
console.log('Before filter:', strArr.length); // prints 4

strArr = strArr.filter(cb);
console.log('After filter:', strArr.length); // prints 2
```



# Type Assertion (Type Casting)
* It is just used to inform the compiler.
* No runtime impact.
* When using with JSX, only **as** syntax is allowed.

```ts
let myvar: any = 'this is a string';
let len: number = (<string>myvar).length;
let len2: number = (myvar as string).length;
```



# Functions
* The number of arguments given to a function has to match the number of parameters the function expects, unless the parameter is optional.

```ts
// named function
function add (x: number, y: number): number {
    return x + y;
}

// anonymous function
let add = function (x: number, y: number): number { return x + y; };
```

## Function Type
* See: [function](#function)

## Optional Parameters
* Parameters can be optional.
* This can be done with question mark(?).
* Required parameters must be placed before the optional parameters within parameter list.

```ts
function buildName (firstName: string, lastName?: string) {
    return ${firstname} + (lastname ? `${lastname}` : ``);
}

buildName('Yigit'); // OK
buildName('Yigit', 'Yuce'); // OK
buildName('Mr.', 'Yigit', 'Yuce'); // ERROR
```

## Default Parameters
* Same as plain javascript.

```ts
function buildName (firstName: string, lastName = 'SURNAME') {
    return `${firstname} ${lastname}`;
}

buildName('Yigit'); // OK, prints: 'Yigit SURNAME'
buildName('Yigit', 'Yuce'); // OK, prints: 'Yigit Yuce'
buildName('Mr.', 'Yigit', 'Yuce'); // ERROR
```

## Rest Parameters (spread)
```ts
function buildRPC (methodName: string, ...params: any[]) {
    return `${methodName}(${params.join(', ')})`;
}

let addRPC: string = buildRPC ('add', 1, 2, 3);
// prints: add(1, 2, 3)
```

## Overloading
* If the function has a different return type according to parameter type you can handle this with function overloading.
* If function call is not matched with any of the overloaded function definitions, this will cause an error.

```ts
let arr: string[] = ['yigit', 'yuce', 'software', 'developer'];
function pick (item: string): number;
function pick (item: number): string;
function pick (item: any): any {
    if (typeof item === 'string') {
        return arr.findIndex((el) => (el === item));
    }
    else if (typeof item === 'number') {
        return (arr.length > item) ? '' : arr[item];
    }
}

pick(1);
pick('yuce');
```



# Interfaces
* can be used to define types of object elements.
* extra property within the object that be sent or assigned 
  * is not an error for interface type
  * is error for plain object type

```ts
// without interface
function print1 (usr:{ id:number, username:string}): void {
    console.log(`${usr.id}: ${usr.username}`);
}

// with interface definition
interface User {
    id: number,
    username: string,
}

function print2 (usr:User): void {
    console.log(`${usr.id}: ${usr.username}`);
}

let usrObj = {
    id: 1,
    username: 'yigityuce',
};

// both valid
print1(usrObj);
print2(usrObj);


let usrObj2 = {
    id: 1,
    username: 'yigityuce',
    email: 'ygtyce@gmail.com',
};

print1(usrObj2); // not valid (unwanted property)
print2(usrObj2); // valid
```


## Optional Properties
* Not all properties of an interface may be required.
* These optional properties are popular when using **option bags** pattern.
  * This option bags can be passed to the set of function that part of the options are used.
* question mark (**?**) have to be used after property key to do optional property.
 
```ts
interface User {
    id: number,
    username: string,
    from?: string
}

function print (usr:User): void {
    let str = `${usr.id}: ${usr.username}`;
    if (usr.from) str += ` from ${usr.from}`;
    console.log(str);
}
```

## Readonly Properties
* only be modifiable when an object is first created.

```ts
interface User {
    readonly id: number,
    username: string,
}

function print (usr:User): void {
    usr.id = 5; // error
    console.log(`${usr.id}: ${usr.username}`);
}

let usr: User = {
    id: 1,
    username: 'yigityuce'
};

print (usr);
```

## Excess Property Checks
* extra property within the object that be sent or assigned 
  * is not an error for interface type
  * is error for plain object type
* Object literals will be checked to detect excess properties when
  * assigning to other variables
  * passing as argument to function
* If an object has any properties that the “target type” doesn’t have, you’ll get an error.
* Getting around these checks can be done with
  * type casting (assertion) to interface type
  * add string index signature

```ts
interface User {
    readonly id: number,
    username: string,
    from?: string,

    // adding string index signature
    [propName: string]: any
}
```

## Function Types
* interfaces can be used to describe function types like below.
```ts
interface SearchFunc {
    (search: string, substr:string): boolean;
}

let mySearch: SearchFunc = function (src: string, substr:string) :boolean {
    // do something
    return false;
}
```

## Hybrid Types
* interfaces can be hybrid type like below.

```ts
interface Value {
    _value: any;
    (v: any): any;
    toString(): string;
    type(): string;
}

function create (): Value {
    let value: Value = <Value> function (v:any) {};
    value._value = null;
    value.toString = function () {}
    value.type = function () {}
    return value;
}

let val: Value = create();
val(10);
val.toString();
val.type();
val._value;

```




## Indexable Types
* The types that have *index signatures*.
* Supported index signature types:
  * number
  * string
* return type of number index signature must be subtype of the return type of string index signature

```ts
interface StringArray {
    [index: number]: string; // number index signature
}

let myArray: StringArray = ['Yigit', 'Yuce'];
```

```ts
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}

// Error: number index return type is not subtype of string index return type
interface NotOkay {
    [index: number]: Animal;
    [index: string]: Dog;
}
```

* index can be readonly

```ts
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ['Yigit', 'Yuce'];
myArray[1] = 'yigityuce'; // error!
```

## Implementing an Interface
* used with **implements** keyword
* Interfaces just describe public properties of class.
* Interfaces could not describe static properties.
  * constructor method is a static function

```ts
interface ClockInterface {
    currentTime: Date;
    setTime (d: Date): void;
}

class Clock implements ClockInterface {
    currentTime: Date = new Date();
    setTime (d: Date) {
        this.currentTime = d;
    }
    constructor (h: number, m: number) {}
}
```

## Extending Interfaces
* used with **extends** keyword
* An interface can extend multiple interfaces.

```ts
interface Size {
    size: string
}

interface Color {
    color: string
}

interface Border extends Size, Color {
    type: string
}

let border: Border = <Border>{
    size: '1px',
    type: 'solid',
    color: 'rgba(0, 0, 0, 0.5)'
}
```




# Classes
```ts
class User {
    id: number,
    username: string,

    constructor (id:number, username:string) {
        this.id = id;
        this.username = username;
    }

    print (): void {
        console.log(`${this.id}: ${this.username}`);
    }
}
```

## Inheritance
* can be done with **extends** keyword.
* derived class constructor must call its base class constructor by calling **super()** method before use any *this* referenece.

```ts
class Animal {
    name: string;
    constructor (name: string) { this.name = name; }
}

class Rhino extends Animal {
    constructor () { super('Rhino'); }
}

let r1: Rhino = new Rhino();
let r2: Animal = new Rhino();

```


## Access Modifiers
* can be public, private, protected.
* If not specified its accesibilty is public.
* Private properties can not be accessed from outside of the class.
* Protected properties can be accessed from inside of the class or inside of the derived class.
* Class constructor can be protected.
  * In this method base class cannot be instantiated from outside of the derived class.

```ts
class Person {
    protected name: string;
    protected constructor (theName: string) { this.name = theName; }
}

class Employee extends Person {
    private department: string;

    constructor (name: string, department: string) {
        super(name);
        this.department = department;
    }

    public print () {
        return `My name is ${this.name} and I work in ${this.department}.`;
    }
}

let yigitE = new Employee('Yigit', 'R&D');
let yigitP = new Person('Yigit'); // Error: The 'Person' constructor is protected
```


## Readonly Modifier
* Class properties can be marked as readonly.
* Readonly properties must be set:
  * when defining property
  * or at the constructor.

```ts
class Dog {
    readonly name: string;
    readonly numberOfLegs: number = 4;
    constructor (theName: string) {
        this.name = theName;
    }
}
let bambam = new Dog('Bambam');
bambam.name = 'Bambam Jr.'; // error! name is readonly.
```

## Parameter Properties
* Used to initialize class variable at constructor's parameter definition stage.
* If constructor parameter is defined with access and/or readonly modifier, the class variable will be declared and initialized with the variable name and value.
* Previous example can be written like below:

```ts
class Dog {
    readonly numberOfLegs: number = 4;
    constructor (public readonly name: string) {}
}
let bambam = new Dog('Bambam');
bambam.name = 'Bambam Jr.'; // error! name is readonly.
```

## Getter & Setter
* No additional specificaton to Javascript ES6 getter & setter syntax.
* See also: 
  * [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get)
  * [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set)

## Static Properties
* Javascript ES6 specification does not have a static variable declaration.
* It only has how to define static methods.
* Typescript allows to define variable with static keyword.

## Abstract
* Abstract classes can be a base class only.
* Abstract classes can not be instantiated directly.
* Abstract classes can have:
  *  methods that have a function body or
  *  abstract methods
* Abstract methods have to be implemented in derived class.
* Access modifiers have to be written before abstact keyword at method definition.

```ts 
abstract class Person {
    constructor (protected name: string) {}
    public printName (): string {
        return `My name is ${this.name}`;
    }
    public abstract print (): string;
}

class Employee extends Person {
    constructor (name: string, private department: string) {
        super(name);
    }

    public printDepartment (): string {
        return `I work in ${this.department}`;
    }

    public print () {
        return `${this.printName()} and ${this.printDepartment()}.`;
    }
}

let emp1:Employee = new Employee('Yigit', 'R&D'); // OK
let emp2:Person = new Employee('Yigit', 'R&D'); // OK
let emp3 = new Person('Yigit'); // Error: The 'Person' is abstract


emp1.print(); // OK
emp1.printName(); // OK
emp1.printDepartment(); // OK

emp2.print(); // OK
emp2.printName(); // OK
emp2.printDepartment(); // Error: Property 'printDepartment' does not exist on type 'Person'.

```

## Constructor Signature
  
```ts
interface A {}

class A1 implements A {}
class A2 implements A {}

function factory(ctor: { new(...args: any[]): {} }): {} {
    return new ctor();
}

let a1 = factory(A1); // type of a1 is A
let a2 = factory(A2); // type of a2 is A
```


# Generics
* It is like the template functions at C++;

```ts
function add<T> (first: T, second: T): T {
    return first + second;
}

// explicitly define type
add<string>('yigit', 'yuce');

// compiler detects type
add(1, 2);
```

## Generic Types

### Generic Function Type
```ts
function add<T> (first: T, second: T): T {
    return first + second;
}

let addFn: <U>(first:U, second:U) => U = add;

addFn('yigit', 'yuce');
addFn(1, 2);
```

### Generic Interface
```ts
function add<T> (first: T, second: T): T {
    return first + second;
}

// applied to all interface
interface addFnInterface<T> {
    (first: T, second: T): T;
}

let addFnStr: addFnInterface<string> = add;
let addFnNumber: addFnInterface<number> = add;

addFnStr('yigit', 'yuce');
addFnNumber(1, 2);
```

```ts
function add<T> (first: T, second: T): T {
    return first + second;
}

interface addFnInterface {
    // just applied to function
    <T>(first: T, second: T): T;
}

let addFn: addFnInterface = add;

addFn('yigit', 'yuce');
addFn(1, 2);
```



### Generic Class
* Type can not be used for static properties.

```ts
class GenericAccumulate<T> {
    result: T;
    add: (value:T) => T;
}

let obj: GenericAccumulate<number> = new GenericAccumulate<number>();
obj.result = 0;
obj.add =  function <T>(value: T) {
    return this.result += value;
};
obj.add(1);
obj.add(2);
obj.add(3);
```

### Generic Constraint
* If the known property of the generic parameter will be used, it can be done with special type that extend the interface.
* Otherwise this will be compiler error.

```ts
interface Lengthwise {
    length: number;
}

function sizeof<T extends Lengthwise> (element: T) {
    return element.length; 
}

sizeof('Yigit Yuce'); // OK
sizeof(41); // Error, number doesn't have a .length property
```



# Enum
* Can be 
  * numeric
  * string

```ts
enum Direction {
    Up,
    Down,
    Left,
    Right
}

enum TimeRef {
    Before = 1,
    After
}

enum UIFramework {
    Vue = 'Vue',
    React = 'React',
    Angular = 'Angular'
}
```

## Computed and Constant Members
* Enum member will be constant when the enum expression can be fully evaulated at compile time.
* Enum member is considered as constant if:
  * no initialized first member
  * a literal enum expr (string or number literal)
  * reference to previously defined constant enum
  * unary operation applied constant enum expr
    * +, -, ~
  * binary operation applied constant enum expr
    * +, -, *, /, %, <<, >>, >>>, &, |, ^

```ts
enum FileAccess {
    // constant members
    None,
    Read = 1 << 1,
    Write = 1 << 2,
    ReadWrite = Read | Write,
    // computed member
    G = '123'.length
}
```

## Enum Member Types
* When all enum members have constant literal enum values, enum members also become types.

```ts
enum ShapeKind {
    Circle,
    Square,
}

interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}

interface Square {
    kind: ShapeKind.Square;
    sideLength: number;
}

let c: Circle = {
    kind: ShapeKind.Square, //    Error!
    radius: 100,
}
```

## Union Enums
* When all enum members have constant literal enum values, enum also become union.
* This means the enum type variable can only be one of the enum members.

```ts
enum Color { RED, GREEN, BLUE }

function paint (c: Color) {
    if ((c === Color.RED) || (c === Color.GREEN)) {
        // error, c can only be one type
    }
}
```

## Reverse Mapping
* Only applicable to number enum members.

```ts
enum Color { RED, GREEN, BLUE }

console.log(Color.GREEN); // prints 1
console.log(Color[Color.GREEN]); // prints 'GREEN'
```

## Const Enums
* For performance improvement.
* Removed after compilation. They will become inline.



# Advanced Types

## Intersection Types
* Combines multiple types into one.
* Mostly used for mixins.


```ts
class Person {
    constructor (public name: string) {}
}

interface Loggable {
    log (msg: string): void;
}

class PersonLogger implements Loggable {
    log (msg: string): void {
        console.log(`My name is ${msg}`);
    }
}

function <First, Second>extend (f: First, s: Second): First & Second {
    let result: Partial<First & Second> = {};

    for (const prop in first) {
        if (first.hasOwnProperty(prop)) {
            (<First>result)[prop] = first[prop];
        }
    }
    for (const prop in second) {
        if (second.hasOwnProperty(prop)) {
            (<Second>result)[prop] = second[prop];
        }
    }
    return <First & Second>result;
}

const yigit = extend(new Person('Yigit'), PersonLogger.prototype);
yigit.log(yigit.name);
```


## Union Types
* A union type describes a value that can be one of several types.
* Vertical bar (|) to separate each type.

```ts
function bool (x: string | number | boolean): boolean {
    if (typeof x === 'string') {
        return (x.toLowerCase() === 'true');
    }
    else if (typeof x === 'number') {
        return (x !== 0);
    }
    else if (typeof x === 'boolean') {
        return x;
    }
    throw new Error(`Unknown.`);
}
```

* Only the common properties at the union types can be accessible.
```ts
class Human {
    go (): void {}
    think (): void {}
}

class Vehicle {
    go (): void {}
    park (): void {}
}

function getRandomItem (): Human | Vehicle {
    // do something
}

let item = getRandomItem();
item.go(); // OK
item.think(); // Error
```

### Discriminated Unions
* Typescript needs to these to discriminate unions for you:
  * Types that have a common, singleton type property — the discriminant.
  * A type alias that takes the union of those types — the union.
  * Type guards on the common property.

```ts
interface Square {
    type: 'square'; // string literal type
    size: number;
}
interface Rectangle {
    type: 'rectangle';
    width: number;
    height: number;
}
interface Circle {
    type: 'circle';
    radius: number;
}

type Shape = Square | Rectangle | Circle;

function area (s: Shape) {
    switch (s.kind) {
        case 'square': return s.size * s.size;
        case 'rectangle': return s.height * s.width;
        case 'circle': return Math.PI * s.radius ** 2;
    }
}
```


### Type Guards
* The problem will be appeared when you need to know the type of the variable.
* For example:

```ts
// Each of these property access will cause an error
if (item.think) {
    item.think();
}
else if (item.park) {
    item.park();
}
```

#### Type Assertion Guard
* One of the solution is type assertion:

```ts
// Each of these property access will cause an error
if ((<Human>item).think) {
    item.think();
}
else if ((<Vehicle>item).park) {
    item.park();
}
```

#### **typeof** Guards
* Useful for primitive types.

```ts
function bool (x: string | number | boolean): boolean {
    if (typeof x === 'string') {
        return (x.toLowerCase() === 'true');
    }
    else if (typeof x === 'number') {
        return (x !== 0);
    }
    else if (typeof x === 'boolean') {
        return x;
    }
    throw new Error(`Unknown.`);
}
```

#### **instanceof** Guards
* Useful for object types.

```ts
if (item instanceof Human) {
    item.think();
}
else if (item instanceof Vehicle) {
    item.park();
}
```


## Nullable Types
* *Null* and *undefined* have a special meanings in typescript.
* These are not the same.
* These are valid for every type unless **--strictNullChecks** flag is set.

```ts
let s = 'foo';
s = null; // error if flag is set

let sn: string | null = 'bar';
sn = null; // ok

sn = undefined; // error if flag is set
```
* *Undefined* type is **always** a member of union for **optional parameters**.

```ts
class C {
    a: number;
    b?: number;
}
let c = new C();
c.a = 12;
c.a = undefined; // error, 'undefined' is not assignable to 'number'
c.b = 13;
c.b = undefined; // ok
c.b = null; // error, 'null' is not assignable to 'number | undefined'
```


## Type Alias
* Like interface
* Differents from interface:
  * can't be implemented
  * can't extend other type

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
```

* Can be generic:
  
```ts
type Container<T> = { value: T };
```

* Can be used at property of itself.

```ts
type List<T> = {
    value: T;
    next: List<T>;
    previous: List<T>;
};
```


## Literal Types

### String Literal Types
```ts
type keybinding = 'ctrl+s' | 'ctrl+c' | 'ctrl+v';
function keypressed (key: keybinding) {
    if (key === 'ctrl+s') {}
    else if (key === 'ctrl+c') {}
    else if (key === 'ctrl+v') {}
    else {
        // error: should not pass null or undefined
    }
}

keypressed('ctrl+s');
keypressed('ctrl+f'); // error
```

* Another use case:

```ts
interface Square {
    kind: 'square'; // string literal type
    size: number;
}
interface Rectangle {
    kind: 'rectangle'; // string literal type
    width: number;
    height: number;
}
interface Circle {
    kind: 'circle'; // string literal type
    radius: number;
}

type Shape = Square | Rectangle | Circle;

function area (s: Shape): number {
    if (s.kind === 'square') return s.size * s.size;
    if (s.kind === 'rectangle') return s.width * s.height;
    if (s.kind === 'circle') return 2 * Math.PI * s.radius;
}
```

### Numeric Literal Types
* Same as string literal types.



## Index Types

```ts
function set<T, K extends keyof T> (obj: T, key: keyof T, value: T[K]): void {
    obj[key] = value;
}

interface Person {
    name: string;
    age: number;
    height: number;
}

let p: Person = <Person>{
    name: 'Yigit',
    age: 27,
    height: 179,
};

set (p, 'name', 'Yigit Yuce'); // ok
set (p, 'weight', 80); // error
```

### Index Type Query Operator
* **keyof** operator returns **union type** with comibanation of object property keys.


### Indexed Access Operator
* **T[K]** operator returns the T typed object's K named property's type.



## Mapped Types
* Create type based on the exist type.
* For ex. create a readonly version of some type.

```ts
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Optional<T> = {
    [P in keyof T]?: T[P];
}

interface Person {
    name: string;
    age: number;
    height: number;
}

type RoPerson = Readonly<Person>;
type OptPerson = Optional<Person>;
```


## Conditional Types
* Selects one of two possible types that pass the **type relationship** test.

```ts
type TypeName<T> =
    T extends string ? 'string' :
    T extends number ? 'number' :
    T extends boolean ? 'boolean' :
    T extends undefined ? 'undefined' :
    T extends Function ? 'function' :
    'object';

type T0 = TypeName<string>;  // 'string'
type T1 = TypeName<'yigit'>;  // 'string'
type T2 = TypeName<true>;  // 'boolean'
type T3 = TypeName<() => void>;  // 'function'
type T4 = TypeName<string[]>;  // 'object'
```

### Predefined Conditional Types

* Defined in **lib.d.ts**


```ts
//Exclude from T those types that are assignable to U.
Exclude<T, U>

//Extract from T those types that are assignable to U.
Extract<T, U>

//Exclude null and undefined from T.
NonNullable<T>

// Obtain the return type of a function type.
ReturnType<T>

// Obtain the instance type of a constructor function type.
InstanceType<T>
```

```ts
type T00 = Exclude<'a'|'b'|'c'|'d', 'a'|'c'|'f'>;  // 'b' | 'd'
type T01 = Extract<'a'|'b'|'c'|'d', 'a'|'c'|'f'>;  // 'a' | 'c'

type T02 = Exclude<string|number|(() => void), Function>;  // string | number
type T03 = Extract<string|number|(() => void), Function>;  // () => void

type T04 = NonNullable<string|number|undefined>;  // string | number
type T05 = NonNullable<(() => string)|string[]|null|undefined>;  // (() => string) | string[]

function f1 (s: string) {
    return { a:1, b:s };
}

class C {
    x = 0;
    y = 0;
}

type T10 = ReturnType<() => string>;  // string
type T11 = ReturnType<(s: string) => void>;  // void
type T12 = ReturnType<(<T>() => T)>;  // {}
type T13 = ReturnType<(<T extends U, U extends number[]>() => T)>;  // number[]
type T14 = ReturnType<typeof f1>;  // { a: number, b: string }
type T15 = ReturnType<any>;  // any
type T16 = ReturnType<never>;  // never
type T17 = ReturnType<string>;  // Error
type T18 = ReturnType<Function>;  // Error

type T20 = InstanceType<typeof C>;  // C
type T21 = InstanceType<any>;  // any
type T22 = InstanceType<never>;  // never
type T23 = InstanceType<string>;  // Error
type T24 = InstanceType<Function>;  // Error

```



# Namespace
* Used for grouping.

```ts
namespace Input {

    function uniqueId (len: number = 8, chars: string = '0123456789'): string {
        function getRandomInt (min: number, max: number): number {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min)) + min;
        }

        let availableChars: string[] = [ ...Array.from(new Set(chars.split(''))) ]; // unique

        let result: string[] = [];
        while (result.length < len) result.push(availableChars[getRandomInt(0, availableChars.length)]);
        return result.join('');
    };

    export enum Types {
        TEXT = 'text',
        CHECKBOX = 'checkbox'
    }

    export interface InputElement {
        type: string;
        id: string;
        getValue (): any;
    }

    export class Text implements InputElement {
        type = Types.TEXT;
        id: string = uniqueId();
        value: string = '';
        getValue (): any {
            return this.value;
        }
    }

    export class Checkbox implements InputElement {
        type = Types.CHECKBOX;
        id: string = uniqueId();
        value: boolean = false;
        getValue (): any {
            return this.value;
        }
    }

    export function generate (t: Types) : InputElement {
        if (t === Types.TEXT) return new Text();
        if (t === Types.CHECKBOX) return new Checkbox();
    }
}

let inp: Input.InputElement = Input.generate(Input.Types.TEXT);
```

## Multifile Namespaces
* Namespace can be splitted into files.
* It is simply named Javascript object that defined in global scope.

```ts
// FILE: input.ts
namespace Input {
    function uniqueId (len: number = 8, chars: string = '0123456789'): string {
        function getRandomInt (min: number, max: number): number {
            min = Math.ceil(min);
            max = Math.floor(max);
            return Math.floor(Math.random() * (max - min)) + min;
        }

        let availableChars: string[] = [ ...Array.from(new Set(chars.split(''))) ]; // unique

        let result: string[] = [];
        while (result.length < len) result.push(availableChars[getRandomInt(0, availableChars.length)]);
        return result.join('');
    };

    export enum Types {
        TEXT = 'text',
        CHECKBOX = 'checkbox'
    }

    export interface InputElement {
        type: string;
        id: string;
        getValue (): any;
    }

    export function generate (t: Types) : InputElement {
        if (t === Types.TEXT) return new Text();
        if (t === Types.CHECKBOX) return new Checkbox();
    }
}
```

```ts
// FILE: input-text.ts

/// <reference path="input.ts" />
namespace Input {
    export class Text implements InputElement {
        type = Types.TEXT;
        id: string = uniqueId();
        value: string = '';
        getValue (): any {
            return this.value;
        }
    }
}
```

```ts
// FILE: input-checkbox.ts

/// <reference path="input.ts" />
namespace Input {
    export class Checkbox implements InputElement {
        type = Types.CHECKBOX;
        id: string = uniqueId();
        value: boolean = false;
        getValue (): any {
            return this.value;
        }
    }
}
```

```ts
// FILE: main.ts

/// <reference path="input.ts" />
/// <reference path="input-text.ts" />
/// <reference path="input-checkbox.ts" />

let inp: Input.InputElement = Input.generate(Input.Types.TEXT);
```

## Ambient Namespaces
* The javascript library that does not have type declarations can be declared with **ambient** namespace.
* Bacause of the declaration file, it should include just type declarations.

```ts
// FILE: mylib.d.ts

declare namespace MyLib {
    export function myFunc (v: any): void;

    export class MyLib {
        function f (s: string): boolean;
    }
}

// defines itself to global object
declare var mylib = MyLib.MyLib;
```


# Modules

* All module systems are valid.
  * UMD
  * AMD
  * CommonJS
* Contains both declaration and code.


## Ambient Modules
* The javascript modules that does not have type declarations can be declared with **ambient** module.
* Each module declarations can be written into seperate files.
* But common practice is using a single large declaration file.
* Module names must be placed into quotes to do import operation.

```ts
// FILE: node.d.ts
declare module 'url' {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}

declare module 'path' {
    export function normalize(p: string): string;
    export function join(...paths: any[]): string;
    export var sep: string;
}
```

```ts
// FILE: main.ts

/// <reference path="node.d.ts"/>
import * as URL from 'url';
let myUrl = URL.parse('http://www.yigityuce.com');
```


# Decorators
* Decorators are functions that called at runtime with some specific arguments.
* Can be attached to:
  * class declaration
  * method
  * accessor (getter & setter)
  * property
  * parameter
* Multiple decorator can be attached.
* Decorators use the form @**expression**, 
* Expression must evaluate to a function that will be called at runtime with information about the decorated declaration.
* It is experimental tool for now.
* **experimentalDecorators** compiler option must be enabled.


## Decorator Factories

## Class Decorators
* Will be applied to the **constructor**.
* Can not be used in 
  * declaration file 
  * any ambient context (such as on a **declare** class)
* Decorator args:
  * Class constructor
* Decorator return value:
  * If it exists it will be used as class declaration.

```ts
function Sealed (constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}

@Sealed
class User {
    name: string;
    constructor (name: string) {
        this.name = name;
    }
    greet () {
        return `Hello ${this.name}`;
    }
}
```

```ts
function LibraryNamed<T extends { new(...args: any[]): {}}> (ctor: T) {
    return class extends ctor {
        get [Symbol.toStringTag] () {
            return 'MySuperLib';
        }
    };
}

@LibraryNamed
class User {
    name: string;
    constructor (name: string) {
        this.name = name;
    }
    greet () {
        return `Hello ${this.name}`;
    }
}

console.log(Object.prototype.toString.call(new User()));
// expected output: "[object MySuperLib]"
```


## Method Decorators

* Will be applied to the **Property Descriptor** for the method.
* Can not be used in 
  * declaration file 
  * any ambient context (such as in a **declare** class)
  * any overload
  * any function that is not exist in class definiton
* Decorator args:
  * Source of the function
    * Class instance (prototype) for member function
    * Class constructor function for static function
  * Function name
  * Member property descriptor
* Decorator return value:
  * If it exists it will be used as function property descriptor.

```ts
function Trace(src: any, fname: string, descriptor: PropertyDescriptor) {
    const isStatic = (typeof src === 'function');
    const srcName = isStatic ? src.name : src.constructor.name;
    const msg = `${srcName}.${fname} [${isStatic ? 'static' : 'member'} function] is called.`;

    const originalFunc = descriptor.value;
    descriptor.value = function (...args: any[]) {
        const retValue: any = originalFunc.call(this, ...args);
        console.log(msg);
        console.log(`Result: ${retValue}`);
        return retValue;
    };
    return descriptor;
}

class Foo {
    constructor() {
    }

    @Trace
    func1() {
        return 'Ctx of func1';
    }

    @Trace
    static func2() {
        return 'Ctx of func2';
    }
}


let obj: Foo = new Foo();
obj.func1();
Foo.func2();
```


## Accessor Decorators 
* Will be applied to the **Property Descriptor** for the accessor.
* Decorator must be applied just one of the accessor (**get** or **set**)
* Can not be used in 
  * declaration file 
  * any ambient context (such as in a **declare** class)
* Decorator args:
  * Source of the accessor
    * Class instance (prototype) for member function
    * Class constructor function for static function
  * Member name
  * Member property descriptor
* Decorator return value:
  * If it exists it will be used as member's property descriptor.

```ts
function Trace(src: any, name: string, descriptor: PropertyDescriptor) {
    const isStatic = (typeof src === 'function');
    const srcName = isStatic ? src.name : src.constructor.name;
    const msg = `${srcName}.${name} [${isStatic ? 'static ' : ''}member]`;

    const originalGetter = descriptor.get;
    const originalSetter = descriptor.set;
    descriptor.get = function () {
        const retValue: any = originalGetter.call(this);
        console.log(`${msg} value is fetched.`);
        console.log(`Result: ${retValue}`);
        return retValue;
    };
    descriptor.set = function (arg: any) {
        console.log(`${msg} value is changed to ${arg}`);
        return originalSetter.call(this, arg);
    };
    return descriptor;
}

class Foo {
    _var1: any = 1;
    static _var2: any = 2;
    constructor() {
    }

    @Trace
    get var1() {
      return this._var1;
    }
    
    set var1(v:any) {
      this._var1 = v;
  }
    
    @Trace
    get var2() {
      return this._var1;
    }
    
    set var2(v:any) {
      this._var1 = v;
    }
}


let obj: Foo = new Foo();
let temp = obj.var1;
obj.var1 = 'yigit';
```