# PropType Validation

`PropType` validation is a feature of React that helps large applications scale by providing typechecking, similar to 
JavaScript extensions such as `Flow` or `TypeScript`, except they only focus on your Component's `props`

To add `propType` validation to your Component you just need to add a `propType` attribute to either your **class**
or **funcitonal** component that contains for each expected prop
1. The name of the prop
2. The type of the prop 
3. If the prop is `required` or optional (default)

**Aside** Originally `propTypes` were a part of the core `react` library but they have since been extracted own to their
own `prop-types` library -- so when you import `PropType` double check your from:

**Class Component**

```javascript 1.8
class Welcome extends Component {

    static propTypes = {
        name: PropTypes.string
    };

    render() {
        return <div>Welcome {this.props.name}!</div>
    }
}
```

**Functional Component**

```javascript 1.8
const Welcome = (props) => {
    return <div>Welcome {props.name}!</div>
}

Welcome.propTypes = {
    name: PropTypes.string
};
```

Now if the `<Welcome>` component was passed a name prop that was not a string:

```<Welcome name={123}/>```

A warning would be rendered in the debug concole:

<cite> Warning: Failed prop type: Invalid prop `name` of type `number` supplied to `Welcome`, expected `string`. </cite>


**NOTE** Proptype validation only occurs in `development` mode if you app was running in `production` mode the validation 
would be trigger and no warnings would appear

**PropTypes**

In addition to the `string` there are many more prop types:


```javascript 1.8
optionalArray: PropTypes.array,
optionalBool: PropTypes.bool,
optionalFunc: PropTypes.func,
optionalNumber: PropTypes.number,
optionalObject: PropTypes.object,
optionalString: PropTypes.string,
optionalSymbol: PropTypes.symbol,

// Anything that can be rendered: numbers, strings, elements or an array (or fragment) containing these types.
optionalNode: PropTypes.node,

// A React element.
optionalElement: PropTypes.element
```

You can enforce certain props must be passed in with `isRequired`

```javascript 1.8
requiredFunc: PropTypes.func.isRequired
```

You can declare a prop type to be an instance of a particular **class**:
```javascript 1.8
optionalMessage: PropTypes.instanceOf(Message)
```

A given prop can accept multiple different Emums or types:

```javascript 1.8
optionalEnum: PropTypes.oneOf(['News', 'Photos'])
```

```javascript 1.8
optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
])
```

For objects and arrays you can even specify the specific types or shapes:
```javascript 1.8
optionalArrayOf: PropTypes.arrayOf(PropTypes.number)
```

```javascript 1.8
optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
})
```


For an exhaustive list of `propTypes` see the [Prop Type Github Repo](https://github.com/facebook/prop-types) or [React's Prop Type Docs](https://reactjs.org/docs/typechecking-with-proptypes.html)


## References and Resources

[ReactJS PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)

[PropType Github](https://github.com/facebook/prop-types)

