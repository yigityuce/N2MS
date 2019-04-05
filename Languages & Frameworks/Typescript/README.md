- [Typescript](#typescript)
- [Type Annotations](#type-annotations)
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
- [Type Assertion (Type Casting)](#type-assertion-type-casting)
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


# Typescript
* File extension is **.ts**
* Plain javascript code can be used in ts file.
* Compile with:

```bash
tsc filename.ts
```

  
# Type Annotations
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
* ???


## object
* represents non-primitive type

```ts
let myObj: object = {
    key1: 'val1',
    key2: 'val2'
};
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

let myArray: StringArray = ["Bob", "Fred"];
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

## Readonly Modifier
* Class properties can be marked as readonly.
* Readonly properties must be set:
  * when defining property
  * or at the constructor.