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
  - [Indexable Types](#indexable-types)
- [Classes](#classes)



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
    from?: string
}

function print (usr:User): void {
    usr.id = 5; // error
    let str = `${usr.id}: ${usr.username}`;
    if (usr.from) str += ` from ${usr.from}`;
    console.log(str);
}

let usr: User = {
    id:1,
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

## Indexable Types
* TODO: write something...





# Classes
```ts
class User {
    id: number,
    username: string,

    constructor (id:number, username:string) {
        this.id = id;
        this.username = username;
    }

}
```