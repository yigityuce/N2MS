
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