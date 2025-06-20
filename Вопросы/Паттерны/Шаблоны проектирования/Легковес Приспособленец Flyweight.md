---
tags:
  - javascript
---
«Легковес — это структурный паттерн проектирования, который позволяет вместить большее количество объектов в отведённую оперативной память за счёт экономного разделения общего состояния объектов между собой, вместо хранения одинаковых данных в каждом объекте.»

Данный паттерн проектирования позволяет ==вместить большее количество объектов в отведённую оперативную память.==
Легковес экономит память, разделяя общее состояние объектов между собой, вместо хранения одинаковых данных в каждом объекте. 

 Шаблон Приспособленец сводит к минимуму использование памяти или вычислительные расходы, разделяя одни данные между множеством подобных объектов. 
 
 Работает примерно как [[Синглтон]] : помещает все создаваемые обьекты в один массив и следить чтобы значения массива были уникальными, не повторялись. 
 
 Т.е. может быть [1,2,3], но НЕ может быть [1,1,1,2].

Шаблон легковес — способ сохранить память, когда мы создаем большое количество похожих объектов.

```js
//создаем класс книга
class Book {
  constructor(title, author, isbn) {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
  }
}

const books = new Map();
const bookList = []
const addBook = (title, author, isbn, availibility, sales) => {
  const book = {
    ...createBook(title, author, isbn),
    sales,
    availibility,
  };

  bookList.push(book);
  return book;
};

//если книга с таким id есть то не создаем новый экземпляр а возврощаем уже созданный
const createBook = (title, author, isbn) => {
  const existingBook = books.has(isbn);

  if (existingBook) {
    return books.get(isbn);
  }

  const book = new Book(title, author, isbn);
  books.set(isbn, book);

  return book;
};

addBook("Harry Potter", "JK Rowling", "AB123", false, 100);
addBook("Harry Potter", "JK Rowling", "AB123", true, 50);
addBook("To Kill a Mockingbird", "Harper Lee", "CD345", true, 10);
addBook("To Kill a Mockingbird", "Harper Lee", "CD345", false, 20);
addBook("The Great Gatsby", "F. Scott Fitzgerald", "EF567", false, 20);


console.log("Total amount of books: ", isbnNumbers.size);
```