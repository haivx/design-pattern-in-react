<h2>Prototype Pattern: Share properties among many objects of the same type</h2>

1. Definitions:
The prototype pattern is a useful way to share properties among many objects of the same type. The prototype is an object that's native to JavaScript, and can be accessed by objects through the prototype chain.

2. Example:

```js
  class Dog {
    constructor (name) {
      this.name =  name
    }

    bark () {
      return `Wooff`
    }
  }
  
  const Dog1 = new Dog("Spark")
  const Dog2 = new Dog("Rex")

  console.log(Dog.prototype) // constructor: f Dog(name) bark f bark()
  console.log(Dog1.__proto__) // constructor: f Dog(name) bark f bark()
```

The value of __proto__ on any instance of the constructor, is a direct reference to the constructor's prototype! Whenever we try to access a property on an object that doesn't exist on the object directly, JavaScript will go down the prototype chain to see if the property is available within the prototype chain.


3. Advantages and disadvantage

- Advantages: 
   * The prototype pattern is very powerful when working with objects that should have access to the same properties. Instead of creating a duplicate of the property each time, we can simply add the property to the prototype, since all instances have access to the prototype object. Since all instances have access to the prototype, it's easy to add properties to the prototype even after creating the instances.
   ```js
    class Dog {
      constructor (name) {
        this.name =  name
      }

      bark () {
        return `Wooff`
      }
    }
    
    const Dog1 = new Dog("Spark")
    const Dog2 = new Dog("Rex")

    Dog.prototype.play = () => console.log('Play')

    Dog1.play()
   ```
   * The term `prototype chain` indicates that there could be more than one step. Indeed! So far, we've only seen how we can access properties that are directly available on the first object that __proto__ has a reference to. However, prototypes themselves also have a __proto__ object!
   ```js
    class Dog {
      constructor (name) {
        this.name =  name
      }

      bark () {
        return `Wooff`
      }
    }
    
    class SuperDog extends Dog {
      constructor(name) {
        super(name)
      }

      fly() {
        return "Flying"
      }
    }

    const Dog1 = new Dog("Spark")
    Dog1.bark();
    Dog1.fly();
   ```
    We have access to the bark method, as we extended the Dog class. The value of __proto__ on the prototype of SuperDog points to
    the Dog.prototype object!

3. Summary: `The prototype pattern allows us to easily let objects access and inherit properties from other objects. Since the prototype chain allows us to access properties that aren't directly defined on the object itself, we can avoid duplication of methods and properties, thus reducing the amount of memory used.`