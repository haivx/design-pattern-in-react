<h2>Provider Pattern: Make data available to multiple child components</h2>

1. Definitions:

- In some cases, we want to make available data to many (if not all) components in an application. Although we can pass data to components using props, this can be difficult to do if almost all components in your application need access to the value of the props.

- We often end up with something called `prop drilling`, which is the case when we pass props far down the component tree. Refactoring the code that relies on the props becomes almost impossible, and knowing where certain data comes from is difficult.

2. Example:

```js
  const DataContext = React.createContext()

  function App = () => {
    const data = {...}

    return (
      <>
        <DataContext.provider value={data}>
          <SideBar />
          <Content />
        </DataContext.provider>
      </>
    )
  }
```


3. Advantages and disadvantage

- Advantages: The Provider pattern is very useful for sharing global data. A common use case for the provider pattern is sharing a theme UI state with many components.
    * We no longer have to manually pass down the data prop to each component! We no longer have to worry about passing props down several levels through components that don't need the value of the props, which makes refactoring a lot easier.
    * Each component can get access to the data, by using the useContext hook. This hook receives the context that data has a reference with, `DataContext` in this case. The `useContext` hook lets us read and write data to the context object. 

 ```js
  function SideBar () {
    const { data } = React.useContext(DataContext);

    return <div>{data.title}</div>
  }
 ```

 - Disadvantage: 