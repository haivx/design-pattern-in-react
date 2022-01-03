<h2>Singleton Pattern: Share a single global instance throughout our application</h2>

1. Definitions:
- Singletons are classess which can be initiated once, and can be access globally. This single instance can be shared throughout our application, which makes Singletons great for managing global state.

2. Example:

```js
  let counter = 0;

  class Counter {
    getInstance() {
      return this
    }

    getCount {
      return counter
    }

    increment {
      return ++counter
    }

    decrement {
      return --counter
    }
  }

  const counter1 = new Counter();
  const Counter2 = new Counter();

  const isEqual = counter1.getInstance() === counter2.getInstance() // fasle

  counter1.increment();

  counter2.increment();
  counter2.increment();
  console.log(counter1.getCount())
  console.log(counter2.getCount())
```

By calling the new method twice, we set the `counter1` and `counter2` to differenct instances.

=> By Export just 1 instance and import in others places in application, we can achieve just one values which shared among all places.

Some reality example: Axios instance, redux instance, and some other global instances if you can see in special files in application.

3. Advantages and disadvantage

a. Advantages: Instead of having to set up memory for new instance each time, we only have to setup memory for one instance. This could potentially save a lot of memory space
b. Disadvantages: However, Singletons actually considered an anti-pattern in javascript. 

+ Dependency hiding: In many case, we don't want reuse the same value among other files by importing the same instances. Because we can accidently modify the values in Singleton. This can lead to unexpected behavior, since multiple instances of the Singleton can be shared throughout the application. which would all get modified as well.

+ Global Behaviour: 
  * A Singleton instance should be able to get referenced throughout the entire app. The global variables essentially show the same behaviour. But this(global behaviour) is considered as a bad design decision. `let` and `const` help us so much.
  * The new module system in JavaScript makes creating globally accessible values easier without polluting the global scope, by being able
to export values from a module, and import those values in other files.
  * However, the common usecase for a Singleton is to have some sort of global state throughout your application. Having multiple parts of your codebase rely on the same mutable object can lead to unexpected behavior.
  * Usually, certain parts of the codebase modify the values within thes global state, whereas others consume that data. The order of execution here is important: we don't want to accidentally consume data first, when there is no data to consume (yet)! Understanding the data flow when using a global state can get very tricky as your application grows, and dozens of components rely on each other.

+ State management in React:
  * In React, we often rely on a global state through state management tools such as Redux or React Context instead of using Singletons
  * Although their global state behavior might seem similar to that of a Singleton, these tools provide a read-only state rather than the mutable state of the Singleton.
  * Although the downsides to having a global state don't magically disappear by using these tools, we can at least make sure that the global state is mutated the way we intend it, since components cannot update the state directly.