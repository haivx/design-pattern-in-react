<h2>Command Pattern: Decouple methods that execute tasks by sending commands to a commander</h2>


1. Definitions: With the Command Pattern, we can decouple objects that execute a certain task from the object that calls the method.

2. Example:
Look an example:
```js
  class OrderManager {
    constructor() {
      this.orders = []
    }

    placeOrder(order, id) {
      this.orders.push(id)
      return `You have successfully ordered ${order} ${id}`
    }

    trackOrder (id) {
      return `Your order ${id} will arrived in 20 minutes`
    }

    cancelOrder(orderId) {
      this.orders = this.orders.filter(id => id !== orderId) 
      return `You have cancelled order ${orderId}`
    }
  }

  const manager = new OrderManager();
  manager.placeOrder("Pad Thai", 30)
```
However, there are downsides to invoking the methods directly on the manager instance. It could happen that we decide to rename certain methods later on, or the functionality of the methods change.

Say that instead of calling it placeOrder, we now rename it to addOrder! This would mean that we would have to make sure that we don't call the placeOrder method anywhere in our codebase, which could be very tricky in larger applications.

Instead, we want to decouple the methods from the manager object, and create separate command functions for each command!

So by using command pattern:
```js
  class OrderManger {
    constructor() {
      this.orders = []
    }
    execute(command, ...args) {
      return command.execute(this.orders, ...args)
    }
  }

  class Command {
    constructor(execute) {
      this.execute = execute
    }

    function PlaceOrderCommand(order, id) {
      return new Command(orders => {
        orders.push(id);
        console.log(`You have successfully ordered ${order} ${id}`)
      })
    }

    function CancerOrderCommand(order, id) {
      return new Command(orders => {
        orders = orders.filter(order => order.id !== id)
        console.log(`You have cancelled order ${orderId}`) 
      })
    }
    const manager = new OrderManager();

    manager.execute(new PlaceOrderCommand("Pad Thai", 30))
  }
```

3. Advantages and disadvantage

- Advantages: The command pattern allows us to decouple methods from the object that executes the operation. It gives you more control if you're dealing with commands that have a certain lifespan, or commands that should be queued and executed at specific times.

- Disadvantage: The use cases for the command pattern are quite limited, and often adds unnecessary boilerplate to an application.