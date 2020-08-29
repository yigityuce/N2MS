
- [Version](#version)
- [Basics](#basics)
  - [Embedding JS into JSX](#embedding-js-into-jsx)
  - [Multiline JSX](#multiline-jsx)
  - [HTML Attributes at JSX](#html-attributes-at-jsx)
- [Components](#components)
  - [Props](#props)
  - [States](#states)
  - [Lifecycles](#lifecycles)
  - [Event Handling](#event-handling)
  - [Conditional Rendering](#conditional-rendering)
  - [List Rendering](#list-rendering)
  - [Fragments](#fragments)
  - [Special Props](#special-props)
    - [props.children](#propschildren)
    - [Render Props](#render-props)
  - [Styling](#styling)
    - [Inline CSS](#inline-css)
    - [CSS-in-JS](#css-in-js)
    - [Styled Components](#styled-components)
    - [CSS Modules](#css-modules)
- [Forms](#forms)
  - [Controlled Component](#controlled-component)
- [Lazy Loading](#lazy-loading)
- [Context](#context)
- [Error Handling (Error Boundary)](#error-handling-error-boundary)
- [Element Refs](#element-refs)
  - [Creating Refs](#creating-refs)
  - [Accessing Refs](#accessing-refs)
  - [Callback Refs](#callback-refs)
- [Hooks](#hooks)
  - [useState](#usestate)
  - [useEffect](#useeffect)
- [React Redux](#react-redux)
  - [Install](#install)
- [Best Practices](#best-practices)
  - [Presentational and Container Components](#presentational-and-container-components)
  - [Wrapping a Component (Higher-Order Components)](#wrapping-a-component-higher-order-components)
  - [Project Initiation](#project-initiation)

# Version
* Document created for React version 16.13.1

# Basics

```jsx
// netiher HTML, nor string
// this is JSX
const element = <h1>Hello, Yigit</h1>;
const domElement = document.getElementById('root');

ReactDOM.render(element, domElement);
```

## Embedding JS into JSX
* can be any javascript expr.

```jsx
const name = 'Yigit YUCE';
const element = <h1>Hello, {name}</h1>;
```


## Multiline JSX
* do it with parantheses.

```jsx
const el = (
    <div>
        {content}
    </div>
);
```

## HTML Attributes at JSX
* do it with javascript style

```jsx
const el = (
    <div className="row" id={contentId}>
        {content}
    </div>
);
```

# Components
* define components with function or class.
* preferred is defining with class.
* Component names have to start with capital letter.


```jsx
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}
```

```jsx
export default class Welcome extends React.Component {
    public render(): ReactNode {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

* use component like below:
```jsx
ReactDOM.render(
    <Welcome name="Sara"/>, 
    document.getElementById('root')
);
```

## Props
* **Read-only**
* Components must **never modify** its own props.
* super() method should **always be called** with props argument at class constructor.
* Props will be set in the class with <pre>this.props</pre>

## States
* State is similar to props, but it is private and fully controlled by the component.
* States are defined at constructor.


```jsx
export default class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = { date: new Date() };
    }

    public render(): ReactNode {
        return (
            <div>
                Time is {this.state.date.toLocaleTimeString()}
            </div>
        );
    }
}

ReactDOM.render(<Clock />, document.getElementById('root'));
```

* States have to be update with setState() function. 
* **Direct update is not valid.**
* New state will be **merged** the original one.

```jsx
this.setState({
    key: 'value'
});
```

* Props and states may be updated asynchronously, you should not rely on their values for calculating the next state.

```jsx
// WRONG
this.setState({
    key: this.state.key + this.props.key,
});
```

* To avoid that you can use function as parameter of setState.
* function's 
  * first argument is the previous state
  * second argument is the props at that time

```jsx
this.setState((state, props) => ({
    key: state.key + props.key,
}));
```



## Lifecycles

* Some of the component lifecycles are:
  * componentDidMount()
  * componentWillUnmount()


```jsx
export default class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = { date:new Date() };
    }

    public componentDidMount(): void {
        this.timer = setInterval(() => {
            this.setState({ date: new Date() });
        }), 1000);
    }

    public componentWillUnmount(): void {
        clearInterval(this.timer);
    }

    public render(): ReactNode {
        return (
            <div>
                Time is {this.state.date.toLocaleTimeString()}
            </div>
        );
    }
}

ReactDOM.render(<Clock />, document.getElementById('root'));
```


![lifecycle](./lifecycle.png)



## Event Handling
* Event names are camelCase.
* Must be binded this to handler.

```jsx
export default class Button extends Raect.Component {
    constructor (props) {
        super (props);
        this.buttonClicked = this.buttonClicked.bind(this);
    }

    public buttonClicked(ev): void {
        console.log(`${this.props.text} button clicked`);
    }

    
    public buttonMouseDown = (ev): void => {
        console.log(`${this.props.text} button mouse down`);
    }

    public render(): ReactNode {
        return (
            <button onClick={ buttonClicked } onMouseDown={ buttonMouseDown }>
                { this.props.text }
            </button>
        );
    }
}

ReactDOM.render(<Button text="Login"/>, document.getElementById('root'));
```



## Conditional Rendering
* Conditional rendering is handled at render function with plain javascript if else statements.
* Can be done with inline if statement.
* Component can be **hidden with returning null** at render function.
* You can use variables to store elements. This can help you conditionally render a **part of the component**.

```jsx
function UserGreeting(props) { return <h1>Welcome back!</h1>; }
function GuestGreeting(props) { return <h1>Please sign up.</h1>; }
function Greeting(props) {
    return props.isLoggedIn ? <UserGreeting /> : <GuestGreeting />;
}

function LoginButton(props) { return <button onClick={props.onClick}>Login</button>; }
function LogoutButton(props) { return <button onClick={props.onClick}>Logout</button>; }

export default class LoginControl extends React.Component {
    constructor (props) {
        super(props);
        this.handleLoginClick = this.handleLoginClick.bind(this);
        this.handleLogoutClick = this.handleLogoutClick.bind(this);
        this.state = { isLoggedIn:false };
    }

    public handleLoginClick(): void { 
        this.setState({isLoggedIn: true}); 
    }

    public handleLogoutClick(): void { 
        this.setState({isLoggedIn: false}); 
    }
    
    public render(): ReactNode {
        const isLoggedIn = this.state.isLoggedIn;
        let button;

        if (isLoggedIn) {
            button = <LogoutButton onClick={this.handleLogoutClick} />;
        } 
        else {
            button = <LoginButton onClick={this.handleLoginClick} />;
        }

        return (
            <div>
                <Greeting isLoggedIn={isLoggedIn} />
                {button}
            </div>
        );
    }
}

ReactDOM.render(<LoginControl />, document.getElementById('root'));
```



## List Rendering
* It is handled by plain javascript (like array.map, for loop etc.) inside the render function.
* **Key attribute** have to be set for each element.
* Keys should be unique among their **siblings**, don’t need to be **globally** unique.

```jsx
export default class List extends React.Component {
    constructor (props) {
        super(props);
    }

    public render(): ReactNode {
        const elements = this.props.elements || []).map((el, i) => {
            return <li id={ el.id } key={ `item_${i}` }> { el.text } </li>;
        });

        return <ul> { elements } </ul>;
    }
}

let elements = [
    {
        id: 'el-1'
        text: 'First Element'
    },
    {
        id: 'el-2'
        text: 'Second Element'
    },
];

ReactDOM.render(<List elements={elements}/>, document.getElementById('root'));
```

## Fragments
* used to group sibling root level elements in the component
* it is like Angular's ```<ng-container> ... </ng-container>```


```jsx
export default class Columns extends React.Component {
    public render(): ReactNode {
        return (
            <React.Fragment>
                <td>Hello</td>
                <td>World</td>
            </React.Fragment>
        );
    }
}


// short syntax
export default class Columns extends React.Component {
    public render(): ReactNode {
        return (
            <>
                <td>Hello</td>
                <td>World</td>
            </>
        );
    }
}
```


## Special Props

### props.children
* Some components don’t know their children ahead of time.
* Use the special ```children``` prop to pass children elements directly into their output

```jsx
export default class FancyBorder extends React.Component {
    constructor(props) {
        super(props);
    }

    public render(): ReactNode {
        return (
            <div className= {'FancyBorder FancyBorder-' + props.color }>
                { props.children }
            </div>
        );
    }
}

export default class WelcomeDialog extends React.Component {
    constructor(props) {
        super(props);
    }

    public render(): ReactNode {
        return (
            <FancyBorder color="blue">
                <h1>Welcome</h1>
                <p> Good to see you again!</p>
            </FancyBorder>
        );
    }
}
```

* Expressions as Children
```jsx
function Item(props) {
    return <li>{props.message}</li>;
}

function TodoList() {
    const todos = ['finish doc', 'submit pr', 'nag dan to review'];
    return (
        <ul>
            {todos.map(m => <Item key={m} message={m} />)}
        </ul>
    );
}
```

* Functions as Children
  * **props.children** works just like any other prop.
  * It can pass any sort of data, not just the sorts that React knows how to render.

```jsx
function Repeat(props) {
    let items = [];
    for (let i = 0; i < props.numTimes; i++) {
        items.push(props.children(i));
    }
    return <div>{items}</div>;
}

function ListOfTenThings() {
    return (
        <Repeat numTimes={ 10 }>
            {(i) => <div key={i}>This is item {i} in the list</div>}
        </Repeat>
    );
}
```

* Ignored Expressions
  * ```false```, ```null```, ```undefined```, and ```true``` are valid children. 
  * They simply **don’t render**. 
  * Those below are same:

```jsx
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

### Render Props
* It is a **technique** for sharing code between React components using a **prop whose value is a function**.
* A component with a render prop takes a function that returns a React element and calls it **instead of implementing its own render logic**.
* a render prop is a function that a component uses to know what to render


```jsx
export default class Cat extends React.Component {
    public render(): ReactNode {
        const mouse = this.props.mouse;
        return (
            <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
        );
    }
}

export default class Mouse extends React.Component {
    constructor(props) {
        super(props);
        this.handleMouseMove = this.handleMouseMove.bind(this);
        this.state = { x: 0, y: 0 };
    }

    public handleMouseMove(event): void {
        this.setState({
            x: event.clientX,
            y: event.clientY
        });
    }

    public render(): ReactNode {
        return (
            <div onMouseMove={this.handleMouseMove}>
                { this.props.render(this.state) }
            </div>
        );
    }
}

export default class MouseTracker extends React.Component {
    public render(): ReactNode {
        return (
            <div>
                <h1>Move the mouse around!</h1>
                <Mouse render={mouse => 
                    <Cat mouse={mouse} />
                }/>
            </div>
        );
    }
}
```
* It’s important to remember that just because the **pattern** is called “**render props**” 
* You **don’t have to use** a prop named render to use this pattern. 
* In fact, any prop that is a function that a component uses to know what to render is technically a “render prop”


## Styling

### Inline CSS
* define styles in javascript way (object with camelCase key)
* then pass it to the component as "style" prop
```tsx
export default class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            style: {
                color: '#000000',
                backgroundColor: '#ffffff'
            }
        };
    }

    public render(): ReactNode {
        return <div style={this.state.style}>Hello world!</div>;
    }
}
```

### CSS-in-JS

```tsx
// TODO: TBD
```

### Styled Components

```tsx
// TODO: TBD
```

### CSS Modules
* A CSS Module is a CSS file in which all class names and animation names are scoped **locally** by default.
* create-react-app project supports [CSS Modules](https://github.com/css-modules/css-modules) alongside regular stylesheets using the ```[name].module.scss``` file naming convention
* You should add custom typings like below to prevent typescript error.

```ts
// filename: declarations.d.ts
declare module '*.scss' {
	const content: { [className: string]: string };
	export = content;
}
```


```scss
// MyComponent.module.scss
.MyComponent {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;

    :global .text {
        background-color: #cccccc;
        color: #ff0000;

        &.green {
            color: green;
        }
    }
}
```

```tsx
// MyComponent.tsx
import styles from './MyComponent.module.scss';

export default class MyComponent extends React.Component {
    public render(): ReactNode {
        return (
            <div style={styles.MyComponent}>
                <span className="text">Hello</span>
                <span className="text green">World!</span>
                <span className="text">yigityuce</span>
            </div>
        );
    }
}
```

# Forms

* If you’re looking for a complete solution including validation, keeping track of the visited fields, and handling form submission, **Formik** is one of the popular choices

## Controlled Component
* HTML form elements maintain their own state and update it based on user input.
* We can combine them by making the React state be the “**single source of truth**”
* Controlled components are:
  * ```<input type="text">```
  * ```<textarea>```
  * ```<select>```
* Controlled components support:
  * **value** prop
  * **onChanged** event
* Multiple input elements can be controlled with using "name" attribute.


```jsx
export default class NameForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            userName: '',
            userSurname: '' 
        };

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    public handleChange(event): void {
        this.setState({ [event.name]: event.target.value });
    }

    public handleSubmit(event): void {
        console.log(
            'User infos are submitted:', 
            this.state.userName, 
            this.state.userSurname
        );
        event.preventDefault();
    }

    public render(): ReactNode {
        return (
            <form onSubmit={ this.handleSubmit }>
                <label>
                    Name:
                    <input type="text" name="userName"
                        value={ this.state.userName } 
                        onChange={this.handleChange} 
                    />
                </label>
                <label>
                    Surname:
                    <input type="text" name="userSurname"
                        value={ this.state.userSurname } 
                        onChange={this.handleChange} 
                    />
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}

```

# Lazy Loading

* The React.lazy function lets you render a dynamic import as a regular component.
* Calls the import when first needed
* The lazy component should then be rendered inside a **Suspense** component
* It allows us to show some **fallback content** while we’re waiting for the lazy component to load.
* React.lazy currently only supports **default exports**.


```jsx
const OtherComponent = React.lazy(() => import('./OtherComponent'));

export default class MyComponent extends React.Component {
    constructor(props) {
        super(props);
    }

    public render(): ReactNode {
        return (
            <div>
                <Suspense fallback={<div>Loading...</div>}>
                    <OtherComponent />
                </Suspense>
            </div>
        );
    }
}
```

# Context
* Context is designed to **share** data that can be considered **“global”** for a **set** of React components.
* Using context, we can avoid passing props through intermediate elements.

```jsx
const ThemeContext = React.createContext('light');

export default class App extends React.Component {
    public render(): ReactNode {
        return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
        );
    }
}

function Toolbar(props) {
    return (
        <div>
            <ThemedButton />
        </div>
    );
}

export default class ThemedButton extends React.Component {
    static contextType = ThemeContext;

    public render(): ReactNode {
        return <Button theme={this.context} />;
    }
}
```

# Error Handling (Error Boundary)

* A JavaScript error in a part of the UI shouldn’t break the whole app.
* Error boundaries are React components that catch JavaScript errors anywhere in their **child component** tree, log those errors, and **display a fallback UI**
* Error boundaries work like a JavaScript ``catch {}`` block, but for components.
* Error boundaries only catch errors in the components **below** them in the tree.
* The error will propagate to the closest error boundary above it
* Error boundaries catch errors during:
  * rendering
  * in lifecycle methods
  * in constructors of the whole tree below them
* Error boundaries do ***not*** catch errors for:
  * Event handlers
  * Asynchronous code (setTimeout etc.)
  * Server side rendering
  * Errors thrown in the error boundary itself (rather than its children)


```jsx
export default class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false };
    }

    static getDerivedStateFromError(error) {
        // Update state so the next render will show the fallback UI.
        return { hasError: true };
    }

    public componentDidCatch(error, errorInfo): void {
        console.error(error, errorInfo);
    }

    public render(): ReactNode {
        if (this.state.hasError) {
            // Fallback UI for error
            return <h1> Something went wrong. </h1>;
        }

        return this.props.children; 
    }
}

export default class App extends React.Component {
    constructor(props) {
        super(props);
    }

    public render(): ReactNode {
        return (
            <ErrorBoundary>
                <MyWidget />
            </ErrorBoundary>
        );
    }
}
```

# Element Refs

## Creating Refs
* Refs are created using ```React.createRef()``` and attached to React elements via the ref attribute.

```jsx
export default class MyComponent extends React.Component {
	private element: React.RefObject<HTMLDivElement> = React.createRef();

    public render(): ReactNode {
        return <div ref={this.element} />;
    }
}
```

## Accessing Refs

```jsx
const node = this.myRef.current;
```

## Callback Refs
* Instead of passing a ref attribute created by createRef(), you pass a function. 
* The function receives the React component instance or HTML DOM element as its argument
* Then it can be stored and accessed elsewhere
* [**!Important**] If the ref callback is defined as an inline function, it will get called **twice** during updates, first with **null** and then again with the DOM element. 


```jsx
export default class CustomTextInput extends React.Component {
    constructor(props) {
        super(props);
        this.textInput = null;
        this.focusTextInput = this.focusTextInput.bind(this);
        this.setTextInputRef = this.setTextInputRef.bind(this);
    }
    
    public focusTextInput(): void {
        if (this.textInput) this.textInput.focus();
    }

    public setTextInputRef(element): void {
        this.textInput = element;
    }

    public componentDidMount(): void {
        this.focusTextInput();
    }

    public render(): ReactNode {
        return (
        <div>
            <input
                type="text"
                ref={this.setTextInputRef}
            />
            <button type="button" onClick={this.focusTextInput}>
                Focus the text input
            </button>
        </div>
        );
    }
}
```



# Hooks

* Hooks are back-compatible.
* Hooks don’t work inside classes — they let you use React without classes
* Hooks allow you to reuse stateful logic without changing your component hierarchy
* You won't be needed to use HOC (higher-order components), render props etc. abstraction layers
* Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data)
* React provides a few built-in Hooks.
* You can also create your own Hooks to reuse stateful behavior between different components.

## useState
* Returns array with two items
  * state variable
  * state variable setter (mutation) function
* Takes an argument which is the initial value of state variable


```jsx
const [count, setCount]: [number, (number) => void ] = setState(0);

console.log(count); // prints 0
setCount(1);
console.log(count); // prints 1
```

## useEffect
* The Effect Hook, useEffect, adds the ability to perform side effects from a function component. 
* It serves the same purpose as **componentDidMount**, **componentDidUpdate**, and **componentWillUnmount** in React classes, **but unified into a single API**
* Runs your “effect” function after flushing changes to the DOM.

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
    const [count, setCount] = useState(0);

    // Similar to componentDidMount and componentDidUpdate:
    useEffect(() => (document.title = `You clicked ${count} times`));

    return <button onClick={() => setCount(count + 1)}>Click me</button>;
}
```


* Effects may also optionally specify how to “clean up” after them by returning a function.

```jsx
import React, { useState, useEffect } from 'react';

function Example() {
    const [isOnline, setIsOnline] = useState(0);

    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    // Similar to componentDidMount and componentDidUpdate:
    useEffect(() => {
        const subscription = SomeService.isOnlineStatus$.subscribe(this.handleStatusChange);
        return () => subscription.unsubscribe();
    });

    if (isOnline === null) {
      return 'Loading...';
    }
    return isOnline ? 'Online' : 'Offline';
}
```


# React Redux

## Install
```sh
npm i react-redux
npm i -D redux-devtools
```




# Best Practices

## Presentational and Container Components

|                | Presentational Components        | Container Components |
| ---            | ---                              | ---                                            |
| Purpose        | How things look (markup, styles) | How things work (data fetching, state updates) |
| Aware of Redux | No                               | Yes                                            |
| To read data   | Read data from props             | Subscribe to Redux state                       |
| To change data | Invoke callbacks from props      | Dispatch Redux actions                         |

## Wrapping a Component (Higher-Order Components)

* Wrap components that have same logic with small differents
* **Transforms** a component into another component
* Improves re-usability

```jsx
function someWrapper(WrappedComponent) {
    return class extends React.Component {
        constructor(props) {
            super(props);
        }

        public componentDidUpdate(prevProps): void {
            console.log('old props:', prevProps);
            console.log('new props:', this.props);
        }

        public render(): ReactNode {
            return <WrappedComponent {...this.props} />;
        }
    }
}
```

* Don’t Mutate the Original Component
* Refs Aren’t Passed Through
* Static Methods Must Be Copied Over

```jsx
function enhance(WrappedComponent) {
    class Enhance extends React.Component {/*...*/}
    // Must know exactly which method(s) to copy :(
    Enhance.staticMethod = WrappedComponent.staticMethod;
    return Enhance;
}
```

## Project Initiation

* Init typescript project & use npm
```sh
npx create-react-app _APPNAME_ --use-npm --template typescript
```

* Integrate default project files like prettier, linter configs etc

```sh
git clone https://github.com/yigityuce/fe-project-config.git
cp -r fe-project-config/* _APPNAME_/
```

* Add sass support

```sh
npm i -D node-sass
```

* Create services, components and pages dir

```
mkdir _APPNAME_/src/components
mkdir _APPNAME_/src/services
mkdir _APPNAME_/src/pages
```