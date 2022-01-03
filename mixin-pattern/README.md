<h2>Mixin Pattern: 
Add functionality to objects or classes without inheritance</h2>


1. Definitions:
A mixin is an object that we can use in order to add reusable functionality to another object or class, without using inheritance. We can't use mixins on their own: their sole purpose is to add functionality to objects or classes without inheritance.

2. Example:

```js
  class Dog {
    constructor(name) {
      this.name = name
    }
  }

  const dogFunc() {
    bark: () => console.log("Wooff!"),
    play: () => console.log("Play!"),
  }

  Object.assign(Dog.prototype, dogFunc)

  const dog1 = new Dog("Rex")

  dog1.bark()
```
We can add the `dogFunc` mixin to the Dog prototype with the Object.assign method. This method lets us add properties to the target object: Dog.prototype in this case. Each new instance of Dog will have access to the the properties of `dogFunc`, as they're added to the Dog's prototype!

Although we can add functionality with mixins without inheritance, mixins themselves can use inheritance!

```js
  const animalFunc() {
    walk: () => console.log("Walk"),
    sleep: () => console.log("Sleep"),
  }

  const dogFunc() {
    bark: () => console.log("Wooff!"),
    play: () => console.log("Play!"),
  }

  Object.assign(dogFunc, animalFunc)
  Object.assign(Dog.prototype, dogFunc)
```
Perfect! Any new instance of Dog can now access the walk and sleep methods as well.