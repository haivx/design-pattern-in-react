<h2>Proxy Pattern: Share a single global instance throughout our application</h2>

1. Definitions:
- With a Proxy object, we get more control over the interactions with certain objects. A proxy object can determine the behavior whenever we're interacting with the object, for example when we're getting a value, or setting a value.

Instead of speaking to that person directly, you'll speak to the proxy person who will represent the person you were trying to reach. The same happens in JavaScript: instead of interacting with the target object directly, we'll interact with the Proxy object.


2. Example:

```js
  cons person = {
    name: "John Doe",
    age: 42,
    nationality: "American",
  }

  const personProxy = new Proxy(person, {})
```
- Instead of interacting with this object directly, we want to interact with a proxy object. In JavaScript, we can easily create a new proxy with by creating a new instance of Proxy.

- The first argument of Proxy is an object that represents the handler. In the handler object, we can define specific behavior based on the type of interaction. Although there are many methods that you can add to the Proxy handler, the two most common ones are get and set:
  • get: Gets invoked when trying to access a property 
  • set: Gets invoked when trying to modify a property

```js
  const personProxy = new Proxy(person, {
    get: (obj, prop) => {
      // get some information
    },
    set: (obj, prop, value) => {
      obj[prop] = value // change prop from obj[prop] to value
    }
  })
  
  personProxy.name = "New name"
  personProxy.age = 22
```

3. Advantages and disadvantage

- Advantages: Proxies are a powerful way to add control over the behavior of an object. A proxy can have various use-cases: it can help with validation, formatting, notifications, or debugging.

  * A proxy can be useful to add validation. A user shouldn't be able to change person's age to a string value, or give him an empty name.
  ```js
      const personProxy = new Proxy(person, {
        get: (obj, prop) => {
          // get some information
        },
        set: (obj, prop, value) => {
          if (prop === "age" and typeof value !== "number") {
            console.log("Invalid age!!!")
            return;
          }
          obj[prop] = value // change prop from obj[prop] to value
        }
      })
      
  ```

- Disadvantage: Overusing the Proxy object or performing heavy operations on
each handler method invocation can easily affect the performance of your application negatively. It's best to not use proxies for performance-critical code.