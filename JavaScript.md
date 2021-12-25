- [Functions](#functions)
  - [Normal functions](#normal-functions)
  - [Function expressions](#function-expressions)
  - [Arrow Functions](#arrow-functions)
    - [Using arrow functions as callback function](#using-arrow-functions-as-callback-function)
- [Objects](#objects)
  - [Object literal](#object-literal)
  - [This keyword](#this-keyword)
    - [This and arrow functions](#this-and-arrow-functions)
- [Document Object Model (DOM)](#document-object-model-dom)
  - [Change class using JS](#change-class-using-js)
- [Event Listeners](#event-listeners)

# Functions

## Normal functions

```javascript
function hello() {
  console.log("hello");
}
```

## Function expressions

- Convenient when passing a function as an argument to another function

```javascript
const square = function (x) {
  return x * x;
};

let result = square(5);
console.log(result); // returns 25

// Passing to another functino

function doSomething(f, arr) {
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    result[i] = f(arr[i]);
  }
  return result;
}

const numbers = [1, 2, 3];

const result = doSomething(square, numbers); // passed square function to another function.
```

## Arrow Functions

- Arrow functions shorter for defining function expressions
- Mostly used while writing callback functions
- Uses the window object as their `this`(_refer object section for this_) keyword rather than itself;

```javascript
const square = (x) => {
  return x * x;
};

// if only single statement in function can just leave out the return statement
const square = (x) => x * x;
```

### Using arrow functions as callback function

```javascript
const people = ["hello", "there", "someone"];

people.forEach(function (person) {
  console.log(person);
});

// using arrow function
people.forEach((person) => {
  console.log(person); // anything to that individual object
});
```

# Objects

## Object literal

```javascript
let user = {
  name: "hello",
  age: 20,
  blogs: ["title 1", "title 2"],
};

console.log(user); // will give the entire user object
console.log(user.age); // 20

// Can also use this notation
console.log(user["age"]); // can be useful if the key is dynamic

let key = "age";
console.log(user[key]);
```

- Can add methods to the object as well.

```javascript
let user = {
  name: "hello",
  age: 20,
  blogs: ["title 1", "title 2"],
  login: function () {
    console.log("the user logged in");
  },
  logout: function () {
    console.log("the user logged out");
  },
};

user.login(); // just like a method call on a string
// will call the function
```

## This keyword

- This keyword refers to the current context / the thing on which the method is called
- Works little diff than java.

```javascript
let user = {
  name: "hello",
  age: 20,
  blogs: ["title 1", "title 2"],
  logBlogs: function () {
    // CAN'T DO THIS
    // console.log(blogs);
    // blogs is not defined
    // using this we can refer to the user object and then its blogs
    console.log(this.blogs);
  },
};
```

### This and arrow functions

- When calling a method on a object if defined using arrow function it will refer to the `window` object and not the actual object on which it was called unlike normal functions.

```javascript
let user = {
  name: "hello",
  age: 20,
  blogs: ["title 1", "title 2"],
  logBlogs: () => {
    // THIS HERE REFERS TO THE WINDOW OBJECT AND NOT THE USER
    // console.log(this.blogs);
  },
};
```

- Can also just do this and will work like a normal function expression

```javascript
let user = {
  name: "hello",
  age: 20,
  blogs: ["title 1", "title 2"],
  logBlogs() {
    // THIS HERE REFERS TO THE WINDOW OBJECT AND NOT THE USER
    // console.log(this.blogs);
  },
};
```

# Document Object Model (DOM)

- Better to use `document.querySelector()` & `document.querySelectorAll()` than the old methods.
  - They return a node list as well which is pretty similar to an array ( can use 0-indexing and also forEach method ).
- `innerText` vs `textContent`
  - `innerText` gives only the visible text
  - `textContent` gives all the text present inside
- To change HTML use `innerHTML` (Always best to avoid using it)
- To get a tag attribute use `element.getAttribute('attributeName')` and to set use `element.setAttribute('attribute','value')`;
- If just want to change style use `selectedElement.style.cssProperty`

```javascript
const para = document.querySelector("p");
para.style.color = "red";
para.style.backgroundColor = "blue";
```

## Change class using JS

```css
.error {
  color: red;
}
.success {
  color: green;
}
```

```javascript
const para = document.querySelector("p");
para.classList.add("error");
para.classList.remove("error");

console.log(para.classList); // will give a array consisiting of classes
```

- If any doubt [refer this video](https://www.youtube.com/watch?v=wKBu_dEaF9E&list=PL4cUxeGkcC9haFPT7J25Q9GRB_ZkFrQAc&index=6)

# Event Listeners
