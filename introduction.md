# JSX brief introduction
### basic jsx
```
const element = <h1>Hello, world!</h1>;
```
### jsx with Embedding Expressions
```
const name = 'Jodie';
const element = <h1>Hello, {name}</h1>;
```
### jsx as a variable
```
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```
### jsx preventing xss
```
const title = response.potentiallyMaliciousInput;
// its safe
const element = <h1>{title}</h1>;
```

# component, props & state
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```
### class component
```
Class Counter extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        }
    }

    plus() {
        this.setState({
            count: this.state.count + 1
        })
    }

    return <div onClick={this.plus}>{this.state.count}</div>
}
```
### function component with hook
```
function Counter() {
    let [count, setCount] = useState(0);
    let plus = setCount(count + 1);

    return <div onClick={plus}>{count}</div>
}
```
###  negative example
```
function Counter() {
    let count = 0
    let plus = () => {count = count + 1};

    return <div onClick={plus}>{count}</div>
}
```
### setState is not synchronization
```
function Counter() {
    let [count, setCount] = useState(0);
    let plus = () => {
        setCount(count + 1);
        // what's the result?
        console.log(count);
    };

    return <div onClick={plus}>{count}</div>
}
```
# Life cycle
```
class BlogView extends React.Component { 
    componentDidMount() { 
        fetchBlog(this.props.id); 
    }
    
    componentDidUpdate(prevProps) { 
        if (prevProps.id !== this.props.id) { 
        fetchBlog(this.props.id)
    }
```
```
function BlogView({ id }) { 
    useEffect(() => { 
        console.log('equal componentDidMount + componentDidUpdate');
        fetchBlog(id); 
    }
    return () => { // componentWillUnmount
     console.log('equal componentWillUnmount'); 
     }
}, [id]); 
```
# Redux

### use redux with high order component
```
import React from "react"
import { connect } from "react-redux"

const increase = () => ({ type: "INCREASE_COUNTER" })

const App = ({ counter, increase }) => {
  return (
    <div>
      The counter is {counter}
      <button onClick={increase}>increase</button>
    </div>
  )
}

const mapStateToProps = (state) => ({ counter: state.counter })
const mapDispatchToProps = {
  increase: () => ({ type: "INCREASE_COUNTER" }),
}

const AppContainer = connect(mapStateToProps, mapDispatchToProps)(App)
```

### use redux with hook
```
import React from "react"
import { useSelector, useDispatch } from "react-redux"

const increase = () => ({ type: "INCREASE_COUNTER" })

const App = (props) => {
  const counter = useSelector((state) => state.counter)
  const dispatch = useDispatch()

  return (
    <div>
      the counter is {counter}
      <button onClick={() => dispatch(increase())}>increase</button>
    </div>
  )
}
```