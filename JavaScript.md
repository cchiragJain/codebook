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
  - [Event with the this keyword](#event-with-the-this-keyword)
  - [Event Object](#event-object)
  - [Event prevent Default](#event-prevent-default)
  - [Event Bubbling](#event-bubbling)

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

- If any doubt [refer this video](https://www.youtube.com/watch?v=wKBu_dEaF9E&list=PL4cUxeGkcC9haFPT7J25Q9GRB_ZkFrQAc&index=6)

- Better to use `document.querySelector()` & `document.querySelectorAll()` than the old methods.
  - They return a node list as well which is pretty similar to an array ( can use 0-indexing and also forEach method ).
- `innerText` vs `textContent`
  - `innerText` gives only the visible text
  - `textContent` gives all the text present inside
- To change HTML use `innerHTML` (Always best to avoid using it)
- To get a tag attribute use `element.getAttribute('attributeName')` and to set use `element.setAttribute('attribute','value')`;
- If just want to change style use `selectedElement.style.cssProperty`
- To create a new element use `document.createElement('element')` and then append it to a parent.

```javascript
const para = document.querySelector("p");
para.style.color = "red";
para.style.backgroundColor = "blue";
```

## Change class using JS

```css
/* CSS Code */
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

# Event Listeners

- `addEventListener()` attaches a event handler to the specified element.
- Much better than using inline events like this:

```javascript
const button = document.querySelector("button");
button.onclick = function () {
  console.log("clicked button");
};
// only this will work while defining it inline without using addEventListener
button.onclick = function () {
  console.log("clicked button again");
};
```

- **Using eventListener**

```javascript
const button = document.querySelector("button");
// any type of callback function will work
// both of them will work now
button.addEventListener("click", function () {
  console.log("clicked");
});
button.addEventListener("click", function () {
  console.log("clicked again");
});
```

## Event with the this keyword

- Will **generally** refer to the the thing the event is called on

```javascript
button.addEventListener("click", colorIt);

function colorIt() {
  // this will refer to the button
  this.style.backgroundColor = "green";
}
```

## Event Object

- There is an event object that is passed to the callback function inside the eventListener which contains useful properties

```javascript
const input = document.querySelector("input");

input.addEventListener("keydown", function (event) {
  // event.key
  // event.code
});

// WILL HAVE DIFFERENT PROPERTIES DEPENDING ON THE TYPE OF EVENT
```

## Event prevent Default

- Default behaviour for a form is to post the data to the action part of the page

```javascript
const form = document.querySelector("form");
form.addEventListener("submit", function (e) {
  // this will stop the default behaviour of the form
  e.preventDefault();
});
```

## Event Bubbling

- Events nested inside will bubble up to the top.

```html
<!-- Clicking on just the button will run all the events -->
<div onclick="alert('div clicked')">
  <p onclick="alert('p clicked')">
    <button onclick="alert('button clicked')"></button>
  </p>
</div>
```

- If want to stop bubbling up inside the event object there is a method `e.stopPropogation()`.
