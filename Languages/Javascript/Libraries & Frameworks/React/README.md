
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
- [Forms](#forms)
  - [Controlled Component](#controlled-component)
- [Lazy Loading](#lazy-loading)
- [Context](#context)
- [Error Handling (Error Boundary)](#error-handling-error-boundary)
- [Forward Refs](#forward-refs)
  - [Creating Refs](#creating-refs)
  - [Accessing Refs](#accessing-refs)
  - [Callback Refs](#callback-refs)
- [Best Practices](#best-practices)
  - [Wrapping a Component (Higher-Order Components)](#wrapping-a-component-higher-order-components)
  - [Project Initiation](#project-initiation)


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
class Welcome extends React.Component {
    render() {
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
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = { date: new Date() };
    }

    render() {
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
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = { date:new Date() };
    }

    componentDidMount () {
        this.timer = setInterval(() => {
            this.setState({ date: new Date() });
        }), 1000);
    }

    componentWillUnmount () {
        clearInterval(this.timer);
    }

    render() {
        return (
            <div>
                Time is {this.state.date.toLocaleTimeString()}
            </div>
        );
    }
}

ReactDOM.render(<Clock />, document.getElementById('root'));
```



## Event Handling
* Event names are camelCase.
* Must be binded this to handler.

```jsx
class Button extends Raect.Component {
    constructor (props) {
        super (props);
        this.buttonClicked = this.buttonClicked.bind(this);
    }

    buttonClicked (ev) {
        console.log(`${this.props.text} button clicked`);
    }

    
    buttonMouseDown = (ev) => {
        console.log(`${this.props.text} button mouse down`);
    }

    render () {
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

class LoginControl extends React.Component {
    constructor (props) {
        super(props);
        this.handleLoginClick = this.handleLoginClick.bind(this);
        this.handleLogoutClick = this.handleLogoutClick.bind(this);
        this.state = { isLoggedIn:false };
    }

    handleLoginClick() { this.setState({isLoggedIn: true}); }
    handleLogoutClick() { this.setState({isLoggedIn: false}); }
    
    render () {
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
class List extends React.Component {
    constructor (props) {
        super(props);
    }

    render () {
        const elements = this.props.elements || []).map((el, i) => {
            return <li id={ el.id } key={ `item_${i}` }> { el.text } </li>;
        });

        return (
            <ul> { elements } </ul>
        );
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
class Columns extends React.Component {
    render() {
        return (
            <React.Fragment>
                <td>Hello</td>
                <td>World</td>
            </React.Fragment>
        );
    }
}


// short syntax
class Columns extends React.Component {
    render() {
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
class FancyBorder extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div className= {'FancyBorder FancyBorder-' + props.color }>
                { props.children }
            </div>
        );
    }
}

class WelcomeDialog extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
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
class Cat extends React.Component {
    render() {
        const mouse = this.props.mouse;
        return (
            <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
        );
    }
}

class Mouse extends React.Component {
    constructor(props) {
        super(props);
        this.handleMouseMove = this.handleMouseMove.bind(this);
        this.state = { x: 0, y: 0 };
    }

    handleMouseMove(event) {
        this.setState({
            x: event.clientX,
            y: event.clientY
        });
    }

    render() {
        return (
            <div onMouseMove={this.handleMouseMove}>
                { this.props.render(this.state) }
            </div>
        );
    }
}

class MouseTracker extends React.Component {
    render() {
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
class NameForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            userName: '',
            userSurname: '' 
        };

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({ [event.name]: event.target.value });
    }

    handleSubmit(event) {
        console.log(
            'User infos are submitted:', 
            this.state.userName, 
            this.state.userSurname
        );
        event.preventDefault();
    }

    render() {
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

class MyComponent extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
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

class App extends React.Component {
    render() {
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

class ThemedButton extends React.Component {
    static contextType = ThemeContext;

    render() {
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
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false };
    }

    static getDerivedStateFromError(error) {
        // Update state so the next render will show the fallback UI.
        return { hasError: true };
    }

    componentDidCatch(error, errorInfo) {
        console.error(error, errorInfo);
    }

    render() {
        if (this.state.hasError) {
            // Fallback UI for error
            return <h1> Something went wrong. </h1>;
        }

        return this.props.children; 
    }
}

class App extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <ErrorBoundary>
                <MyWidget />
            </ErrorBoundary>
        );
    }
}
```

# Forward Refs

## Creating Refs
* Refs are created using ```React.createRef()``` and attached to React elements via the ref attribute.

```jsx
class MyComponent extends React.Component {
    constructor(props) {
        super(props);
        this.myRef = React.createRef();
    }

    render() {
        return <div ref={this.myRef} />;
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
class CustomTextInput extends React.Component {
    constructor(props) {
        super(props);
        this.textInput = null;
    }
    
    focusTextInput: () => {
        if (this.textInput) this.textInput.focus();
    }

    setTextInputRef: (element) => {
        this.textInput = element;
    }

    componentDidMount() {
        this.focusTextInput();
    }

    render() {
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


# Best Practices

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

        componentDidUpdate(prevProps) {
            console.log('old props:', prevProps);
            console.log('new props:', this.props);
        }

        render() {
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