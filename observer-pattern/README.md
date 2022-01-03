<h2>Observer Pattern: Use observables to notify subscribers when an event occurs</h2>

1. Definitions:

- With the observer pattern, we can subscribe certain objects, the observers, to other another object, called the observable. Whenever an event occurs, the observable notifies all its observers!

- An observable object usually contains 3 important parts:
    * observers: an array of observers that will get notified whenever a specific event occurs
    * subscribe():a method in order to add observers to theo bservers list
    * unsubscribe(): a method in order to remove observers from the observers list
    * notify():a method to notify all observers whenever as pecific event occurs

2. Example:

- First: Create Observable
```js
  class Observable {
    constructor () {
      this.observer = []
    }

    subscribe(func) {
      this.observer.push(func);
    }

    unSubscribe(func) {
      this.observer = this.observer.filter(subscribe => subscribe !== func)
    }

    notify(data) {
      this.obsevers.forEach(observer => observer(data))
    }
  }

  export default new Observable()
```
- Using in component:
```js
  import { ToastContainer, toast } from "react-toastify"

  function logger(data) {
    console.log(data)
  }

  observable.subscribe(logger)

  function App() {
    function handleClick() {
      observable.notify('User clicked button!')
    }

    return (
      <div>
        <Button onClick={handleClick}>Click me!</Button>
      </div>
    )
  }

```

Whenever a user interacts with either of the components, both the logger function will get notified with the data that we passed to the notify method!

`Although we can use the observer pattern in many ways, it can be very useful when working with asynchronous, event-based data. Maybe you want certain components to get notified whenever certain data has finished downloading, or whenever users sent new messages to a message board and all other members should get notified.`

3. Advantages and disadvantage

- Advantages: 
    * Using the observer pattern is a great way to enforce separation of
concerns and the single-responsiblity principle. 
    * The observer objects aren’t tightly coupled to the observable object, and can be (de)coupled at any time. The observable object is responsible for monitoring the events, while the observers simply handle the received data.

- Disadvantages:
    * If an observer becomes too complex, it may cause performance issues when notifying all subscribers.

4. Case study: observable pattern is RxJS