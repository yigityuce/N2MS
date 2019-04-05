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
- [Interfaces](#interfaces)
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





# Interfaces
```ts
interface User {
    id: number,
    username: string,

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

}
```