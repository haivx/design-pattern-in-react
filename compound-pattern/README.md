<h2>Compound Pattern: Create multiple components that work together to perform a single task</h2>



1. Definitions:
In our application, we often have components that belong to each other. They're dependent on each other through the shared state, and share logic together. You often see this with components like select, dropdown components, or menu items. The compound component pattern allows you to create components that all work together to perform a task.

2. Use case

a. Context API

b. `React.Children.map`: We can also implement the Compound Component pattern by mapping over the children of the component. We can add the open and toggle properties to these elements, by cloning them with the additional props.