<h2>Flyweight Pattern: Reuse existing instances when working with identical objects</h2>

1. Definitions:

The flyweight pattern is a useful way to conserve memory when we're creating a large number of similar objects

2. Example:

Let see an example below:
```js

  class Book {
    contructor(title, author, isbn) {
      this.title = title
      this.author = author
      this.isbn = isbn
    }
  }

  const isNumbers = new Set()
 
  const createBook = (title, author, isbn) => {
    const book = isNumbers.has(isbn)

    if (book) return book;

    const newBook = new Book(title, author, isbn)

    isNumbers.add(isbn);

    return newBook;
  }
``` 
The `createBook` function helps us create new instances of one type of book. However, a library usually contains multiple copies of the same book! Let's create an `addBook` function, which allows us to add multiple copies of the same book. It should invoke the `createBook` function, which returns either a newly created Book instance, or returns the already existing instance.

In order to keep track of the total amount of copies, let's create a bookList array that contains the total amount of books in the library.

```js
  const bookList = []

  const addBook = (title, author, isbn, availibility, sales) => {
    const book = {
      ...createBook(title, author, isbn),
      availibility,
      sales
    }
    bookList.push(book)

    return book;
  }
```

Perfect! Instead of creating a new Book instance each time we add a copy, we can effectively use the already existing Book instance for that particular copy. Every time when invoke `addBook`, we always check if book's instance is existed or not, if not, create new one, otherswise, use existing ones;

  ```js
   const book = isNumbers.has(isbn) 
   if (book) return book;
  ```

Let's create 5 copies of 3 books: Harry Potter, To Kill a Mockingbird, and The Great Gatsby.

```js
  addBook("Harry potter", "JK Rowling", "AB123", false, 100 )
  addBook("Harry potter", "JK Rowling", "AB123", false, 200 )
  addBook("To Kill a Mockingbird", "Happer Lee", "AA123", true, 200 )
  addBook("To Kill a Mockingbird", "Happer Lee", "AA123", false, 400 )
  addBook("The Great Gatsby", "F.Scott", "XX123", false, 200 )

  console.log("Total amount of books /instances", isNumbers.size)
  console.log("Total amount of copies", bookList.length)
```
Although there are 5 copies, we only have 3 Book instances!


3. Advantages and disadvantage

- Advantages: The flyweight pattern is useful when you're creating a huge number of objects, which could potentially drain all available RAM. It allows us to minimize the amount of consumed memory.

In JavaScript, we can easily solve this problem through prototypal inheritance. Nowadays, hardware has GBs of RAM, which makes the flyweight pattern less important.

In some algorithms, you can see using hash table can help us solving problem so quickly 