<h2>Render Pattern: Pass JSX elements to components through props</h2>

1. Definitions:
  A render prop is a prop on a component, which value is a function that returns a JSX element. The component itself does not render anything besides the render prop. Instead, the component simply calls the render prop, instead of implementing its own rendering logic.


2. Example:

 Normal way:

```js
  export default function App () {
    const [value, setValue] = useState();

    return (
      <div>
        <h1>Temperature Converter</h1>
        <Input value={value} handleChange={setValue}>
        <Kelvin value={value} />
        <Fahrenheit value={value} />
      </div>
    )
  }
```

Render props way:
```js
  const Input(props) {
    const [value, setValue] = useState();
    return (
      <>
        <Input value={value} handleChange={setValue}>
        {props.render(value)}
      </>
    )
  }

  export default function App () {
    return (
      <div>
        <h1>Temperature Converter</h1>
        <Input 
            render={(value) => {
              <>
                  <Kelvin value={value} />
                  <Fahrenheit value={value} />
              </>
            }}
          >

      </div>
    )
  }
```

Perfect, the Kelvin and Fahrenheit components now have access to the value of the user's input!

<h4>Notes</h4>
Besides regular JSX components, we can pass functions as children to React components. This function is available to us through the children prop, which is technically also a render prop.

We have access to this function, through the props.children prop that's available on the Input component. Instead of calling props.render with the value of the user input, we'll call props.children with the value of the user input.

```js
  const Input(props) {
    const [value, setValue] = useState();
    return (
      <>
        <Input value={value} handleChange={setValue}>
        {props.children(value)}
      </>
    )
  }

  export default function App () {
    return (
      <div>
        <h1>Temperature Converter</h1>
        <Input>
          {(value) => {
              <>
                  <Kelvin value={value} />
                  <Fahrenheit value={value} />
              </>
            }}
        </Input>
      </div>
    )
  }
```

3. Advantages and disadvantage

- Advantages:
   * Sharing logic and data among several components is easy with the render props pattern. Components can be made very reusable, by using a render
or children prop. Although the Higher Order Component pattern mainly solves the same issues, namely reusability and sharing data, the render props pattern solves some of the issues we could encounter by using the HOC pattern.
  * The issue of naming collisions that we can run into by using the HOC pattern no longer applies by using the render props pattern, since we don't automatically merge props. We explicitly pass the props down to the child components, with the value provided by the parent component.
  * Since we explicitly pass props, we solve the HOC's implicit props issue. The props that should get passed down to the element, are all visible in the render prop's arguments list. This way, we know exactly where certain props come from.
  * We can separate our app's logic from rendering components through render props. The stateful component that receives a render prop can pass the data onto stateless components, which merely render the data.

- Disadvantage:
  * The issues that we tried to solve with render props, have largely been replaced by React Hooks. As Hooks changed the way we can add reusability and data sharing to components, they can replace the render props pattern in many cases.
  * Since we can't add lifecycle methods to a render prop, we can only use it on components that don't need to alter the data they receive.