
# BASICS

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

# COMPONENTS
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
* Components must never modify its own props.
* super() method should always be called with props argument at class constructor.
* Props will be set in the class with <pre>this.props</pre>

## States
* State is similar to props, but it is private and fully controlled by the component.
* States are defined at constructor.


```jsx
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = { date:new Date() };
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
* Direct update is not valid.
* New state will be merged the original one.

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
        this.timer = setInterval((function () {
            this.setState({ date:new Date() });
        }).bind(this), 1000);
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

    render () {
        return (
            <button onClick={buttonClicked}>
                {this.props.text}
            </button>
        );
    }
}

ReactDOM.render(<Button text="Login"/>, document.getElementById('root'));
```



## Conditional Rendering
* Conditional rendering is handled at render function with plain javascript if else statements.
* Can be done with inline if statement.
* Component can be hidden with returning null at render function.

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
* Key attribute have to be set for each element.

```jsx
class List extends React.Component {
    constructor (props) {
        super(props);
    }

    render () {
        return (
            <ul>
            {
                (this.props.elements || []).map((el, i) => {
                    return (
                        <li id={el.id} key={`item_${i}`}>
                            {el.text}
                        </li>
                    )
                });
            }
            </ul>
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