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
- [Asynchronous JavaScript](#asynchronous-javascript)
  - [HTTP Requests](#http-requests)
  - [Callback functions](#callback-functions)
  - [JSON Data](#json-data)
  - [Callback hell](#callback-hell)
  - [Promises](#promises)
    - [Promises Example](#promises-example)
    - [Chaining Promises](#chaining-promises)
  - [Fetch API](#fetch-api)
  - [Async & Await](#async--await)

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

// will remove if already there and add if not present
para.classList.toggle("className");
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

# Asynchronous JavaScript

- Governs How we perform tasks that which take some time to complete ( ex. getting data from a database )
- Javascript is single threaded by nature

## HTTP Requests

- Make requests to get data from another server
- Requests are made to API endpoints
- Make a `HTTP Request` to the api endpoint and get data back in `JSON format`

- **Old method was to create a `XMLHttpRequest()`**

```javascript
const request = new XMLHttpRequest();
request.addEventListener("readystatechange", () => {
  // when request.readyState === 4 then the request is completed
  // does not mean however that we got the resultant data or there may be some error as well
  if (request.readyState === 4 && request.status === 200) {
    console.log(request.responseText);
  }
});
request.open("GET", "endpoint");
request.send();
```

## Callback functions

```javascript
const getSomething = (callback) => {
  const request = new XMLHttpRequest();
  request.addEventListener("readystatechange", () => {
    // when request.readyState === 4 then the request is completed
    // does not mean however that we got the resultant data or there may be some error as well
    if (request.readyState === 4 && request.status === 200) {
      callback(undefined, request.responseText);
    } else if (request.readyState === 4) {
      callback("could not get the data", undefined);
    }
  });
  request.open("GET", "endpoint");
  request.send();
};

getSomething(
  // a callback function
  // convention error first and then data second
  (err, data) => {
    console.log("callback fired");
    if (err) {
      console.log(err);
    } else {
      console.log(data);
    }
  }
);
```

```javascript
// can pass endpoint in the resource property
const getSomething = (resource, callback) => {
  const request = new XMLHttpRequest();
  request.addEventListener("readystatechange", () => {
    if (request.readyState === 4 && request.status === 200) {
      callback(undefined, request.responseText);
    } else if (request.readyState === 4) {
      callback("could not get the data", undefined);
    }
  });
  request.open("GET", resource);
  request.send();
};

getSomething("someEndpoint", (err, data) => {
  console.log("callback fired");
});
```

- The above code will start and then finish later aside from the normal thread of js

## JSON Data

- The JSON data that we get is actually a string and can be converted to a JS array of objects
- JSON use is to transfer data b/w server and client

```javascript
// inside a event listener
const data = JSON.parse(request.responseText);
```

- JSON data key needs to be in a string and value as well
  - numbers and booleans are fine

```json
[{ "text": "somethingn", "author": "shaun", "age": 20, "value": true }]
```

```json
[
  { "text": "somethingn", "author": "shaun", "age": 20, "value": true },
  { "text": "there", "author": "hello", "age": 19, "value": false }
]
```

## Callback hell

- Callback within a callback within a callback...

```javascript
// can pass endpoint in the resource property
const getSomething = (resource, callback) => {
  const request = new XMLHttpRequest();
  request.addEventListener("readystatechange", () => {
    if (request.readyState === 4 && request.status === 200) {
      callback(undefined, request.responseText);
    } else if (request.readyState === 4) {
      callback("could not get the data", undefined);
    }
  });
  request.open("GET", resource);
  request.send();
};

// WORKS -> WILL ONLY CALL THE INNER ONE AFTER OUTER IS COMPLETED
// EVEN THOUGH THIS WORKS THIS CAN GET MESSY REALLY FAST
getSomething("someEndpoint", (err, data) => {
  console.log(data);
  getSomething("someEndpoint", (err, data) => {
    console.log(data);
    getSomething("someEndpoint", (err, data) => {
      console.log(data);
    });
  });
});
```

## Promises

- Promise is basically something that will take some time to do and will lead to two things either it is resolved(success) or rejected(failed).

```javascript
const doSomething = () => {
  // resolve and reject are built into Promise API and will recieve as parameters
  return new Promise((resolve, reject) => {
    // fetch some data
    resolve(data);
    // reject(error);
  });
};
```

```javascript
// then will take another callback method and will fire it when promise gets resolved or rejected
// first one will be fired when resolved
// second one will be fired when rejected
doSomething().then(
  (data) => {
    console.log(data);
  },
  (error) => {
    console.log(error);
  }
);
```

- Better way to do this is to use `catch`

```javascript
doSomething()
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  });
```

### Promises Example

```javascript
const getSomething = (resource) => {
  return new Promise((resolve, reject) => {
    const request = new XMLHttpRequest();
    request.addEventListener("readystatechange", () => {
      if (request.readyState === 4 && request.status === 200) {
        resolve(data);
      } else if (request.readyState === 4) {
        reject("some error has occured");
      }
    });
    request.open("GET", resource);
    request.send();
  });
};

getSomething("endpoint")
  .then((data) => {
    console.log(data);
  })
  .catch((err) => {
    console.log(err);
  });
```

### Chaining Promises

```javascript
getSomething("promise1Endpoint")
  .then((data) => {
    console.log("promise 1", data);
    // this returns a promise so we can chain another then to this function
    return getSomething("promise2Endpoint");
  })
  .then((data) => {
    console.log("promise 2", data);
    return getSomething("promise3Endpoint");
  })
  .then((data) => {
    console.log("promise 2", data);
  })
  .catch((err) => {
    // single catch wiil work for all of them
    console.log("error", error);
  });
```

## Fetch API

- Easier and newer way to make requests as compared to `XMLHttpRequest`.
- Uses the promise API
- However only rejects when there is a network error
- Use status codes to check if succes or not

```javascript
fetch("someEndpoint")
  .then((response) => {
    console.log("resolved", response);
    // will return a promise and parse the json itself
    return response.json();
  })
  .then((data) => {
    console.log(data);
  })
  .catch((error) => {
    console.log("rejected", error);
  });
```

## Async & Await

- Async function will always return a promise
- What await does is to stop JS ( function is already async so will not stop the normal calls ) and return value only when the promise gets resolved
- **Non blocking code**

```javascript
const getSomething = async () => {
  const response = await fetch("someEndpoint");
  // response.json() also returns a promise
  const data = await response.json();

  return data;
};

// BUT WILL STILL NEED TO USE A SINGLE .then() when the function gets called since async functions return promises

getSomething().then(data => console.log("resolved", data););
```
