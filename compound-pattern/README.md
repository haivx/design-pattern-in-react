<h2>Compound Pattern: Create multiple components that work together to perform a single task</h2>


1. Definitions:
In our application, we often have components that belong to each other. They're dependent on each other through the shared state, and share logic together. You often see this with components like select, dropdown components, or menu items. The compound component pattern allows you to create components that all work together to perform a task.

2. Use case

a. Context API

Let's look at an example: we have a list of squirrel images! Besides just showing squirrel images, we want to add a button that makes it possible for the user to edit or delete the image. We can implement a FlyOut component that shows a list when the user toggles the component.
Within a FlyOut component, we essentially have three things:
• The FlyOut wrapper, which contains the toggle button and the list
• The Toggle button, which toggles the List
• The List , which contains the list of menu items

Using the Compound component pattern with React's Context API is perfect for this example!

```js
  const FlyOutContext = createContext();

  function FlyOut(props) {
    const [open, toggle] = useState(false)

    const providerValue = { open, toggle }

    return (
      <FlyOutContext.Provider value={providerValue}>
        {props.children}
      </FlyOutContext.Provider>
    )
  }

  function Toggle() {
    const { open, toggle } = useContext(FlyOutContext);

    return (
      <div onClick={() => toggle(!open)}>
        <Icon />
      </div>
    )
  }
// However, we can also make the Toggle component a property of the FlyOut component!
  FlyOut.Toggle = Toggle 
```
This means that if we ever want to use the FlyOut component in any file, we only have to import FlyOut!

```js
  import React from 'react';
  import { Flyout } from "./Flyout";

  export default FlyoutMenu() {
    return (
      <Flyout>
        <FLyout.Toggle />
      </Flyout>
    )
  }
```

Just a toggle is not enough. We also need to have a List with list items, which open and close based on the value of open.

```js
  function List({ children }) {
    const { open } = useContext(FlyOutContext);

    return open && <ul>{children}</ul>
  }

  function Item({ children }) {
    return <li>{children}</li>
  }

  FlyOut.List = List
  FlyOut.Item = Item
```

We can now use them as properties on the FlyOut component! In this case, we want to show two options to the user: Edit and Delete. 

```js
  import React from 'react';
  import { Flyout } from "./Flyout";

  export default FlyoutMenu() {
    return (
      <FlyOut>
        <FLyOut.Toggle />
        <FlyOut.List>
          <FlyOut.Item>Edit</FlyOut.Item>
          <FlyOut.Item>Delete</FlyOut.Item>
        </FlyOut.List>
      </FlyOut>
    )
  }
```
Perfect! We just created an entire FlyOut component without adding any state in the FlyOutMenu itself!
The compound pattern is great when you're building a component library. You'll often see this pattern when using UI libraries like Semantic UI.

b. `React.Children.map`: We can also implement the Compound Component pattern by mapping over the children of the component. We can add the open and toggle properties to these elements, by cloning them with the additional props.

```js
  function FlyOut(props) {
    const [open, toggle] = useState(false);

    return (
      // All children components are cloned, and passed the value of open and toggle. Instead of having to use the Context API like in the previous example, we now have access to these  
      // two values through props.
      <>
        {React.Children.map(props.children, (child) => React.cloneElement(child, { open, toggle }))}
      </>
    )
  }

  function Toggle({ open, toggle }) {
    return (
      <div onClick={() => toggle(!open)}>
        <Icon />
      </div>
    )
  }

  function List({ children, open, toggle }) {
    return open && <ul>{children}</ul>
  }

  function Item({ children, open, toggle }) {
    return <li>{children}</li>
  }

  FlyOut.Toggle = Toggle
  FlyOut.List = List
  FlyOut.Item = Item
```


3. Advantages and disadvantage

- Advantages: 
    * Compound components manage their own internal state, which they share among the several child components. When implementing a compound component, we don't have to worry about managing the state ourselves.
    * When importing a compound component, we don't have to explicitly import the child components that are available on that component.
      ```js
      FlyOut.Toggle = Toggle
      FlyOut.List = List
      FlyOut.Item = Item
      ```
- Disadvantage: 

    * When using the React.children.map to provide the values, the component nesting is limited. Only direct children of the parent component will have access to the open and toggle props, meaning we can't wrap any of these components in another component.
    ```js
    export default FlyoutMenu() {
      return (
        <FlyOut>
          <div>
          // This will breaks
            <FLyOut.Toggle />
            <FlyOut.List>
              <FlyOut.Item>Edit</FlyOut.Item>
              <FlyOut.Item>Delete</FlyOut.Item>
            </FlyOut.List>
          </div>
        </FlyOut>
      )
    }
    ```
    * Cloning an element with React.cloneElement performs a shallow merge. Already existing props will be merged together with the new props that we pass. This could end up in a naming collision, if an already existing prop has the same name as the props we're passing to the React.cloneElement method. As the props are shallowly merged, the value of that prop will be overwritten with the latest value that we pass.