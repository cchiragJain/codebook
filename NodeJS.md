- [Introduction](#introduction)
  - [Running files](#running-files)
- [Nodejs basics](#nodejs-basics)
  - [Global object](#global-object)
  - [Modules & Require](#modules--require)
  - [Core Modules](#core-modules)
    - [OS](#os)
    - [File system](#file-system)
      - [Reading Files](#reading-files)
      - [Writing files](#writing-files)
      - [Directories](#directories)
      - [Deleting files](#deleting-files)
    - [Streams & Buffer](#streams--buffer)
- [Client & Servers](#client--servers)
- [Requests & Responses](#requests--responses)
  - [Request Object](#request-object)
  - [Response Object](#response-object)

# Introduction

- Node allows us to run javascript code on to the server/directly on computers using the V8 engine.
- Adds more feature to the normal javascript as well.
- But also lose normal JS features like DOM etc.( not really needed on the server )
  ![](diagrams/noderole.drawio.png)

## Running files

```bash
node filename.js
```

# Nodejs basics

## Global object

- Inside the browser `window` is the global object
- In node the global object is `global` which does not represent the browser.
- No need to specify the global object

```javascript
console.log(__dirname); // will output the current directory
console.log(__filename); // will output the current filename
```

- Normal window methods like document etc don't work

## Modules & Require

- When we `require` a file node will look for that file and then also run it as well.

```javascript
// File
const people = ["a", "b", "c", "d"];
console.log(people);
```

- Running main file will log the people array.

```javascript
// Main file
const xyz = require("./file");
```

- `xyz` here is an empty object and we don't have direct access to the people array inside the main file
- require will only run the file
- In order to export var we can use `module.exports` which will give value to `xyz`

```javascript
// File
const people = ["a", "b", "c", "d"];
console.log(people);

module.exports = "hello";
// xyz in the main file will get this value
```

- Can export multiple as well using a object
- Better to use destrucutre syntax

```javascript
// Main file
const { people, ages } = require("./file");
// can directyle access people and ages now
```

## Core Modules

### OS

- Refers to the operating system

```javascript
const os = require("os");
```

- Useful methods

```javascript
os.platform(); // current os
os.homedir(); // user home directory
```

### File system

#### Reading Files

- Async function requires a callback
- `readFile`

```javascript
const fs = require("fs");
fs.readFile("./docs/blog.txt", (err, data) => {
  if (err) {
    console.log(err);
  }
  console.log(data); // gives a buffer
  console.log(data.toString()); // will convert data to string
});
```

#### Writing files

- Async
- `writeFile`

```javascript
// path,content,callback
fs.writeFile("./docs/blog.txt", "hello world", () => {
  console.log("file was written");
});
```

- Will create file if does not exist

```javascript
fs.writeFile("./docs/blog2.txt", "hello again", () => {
  console.log("file was created and then written");
});
```

#### Directories

- Async
- `mkdir` & `rmdir`
- `existsSync` is synchronous (will stop the code)

```javascript
if (!fs.existsSync("./assets")) {
  // will only run if does not exist since mkdir gives error if already exists
  fs.mkdir("./assets", (err) => {
    if (err) {
      console.log(err);
    }
    console.log("created");
  });
} else {
  // if exists delete it
  fs.rmdir("./assets", (err) => {
    if (err) {
      console.log(error);
    }
    console.log("deleted");
  });
}
```

#### Deleting files

- Async
- `unlink`

```javascript
// only if exists
if (fs.existsSync("./docs/deleteme.txt")) {
  fs.unlink("./docs/deleteme.txt", (err) => {
    if (err) {
      console.log(err);
    }
    console.log("deleted");
  });
}
```

### Streams & Buffer

- When there is a large file we could wait and just read it first and do something with it later
- But could also use a stream of data by which we can start using the data before it has fully loaded
- The data gets send in small packets known as `buffer` through the `stream` so everytime we get a new chunk of data from the source we can start using it
- ex. streaming netflix ( the whole video is not send at a single time and only some of it is send)

- **READSTREAM**
- Read from a file
- readStream is kind of like a event handler like in front end js
- consider data to be an event
- everytime we get a new chunk of data we fire the callback

```javascript
// filelocation,(optional) encoding we want the data to be in otherwise buffer
const readStream = fs.createReadStream("./docs/largefile.txt", {
  encoding: "utf-8",
});

readStream.on("data", (chunk) => {
  console.log("----- NEW CHUNK -----");
  console.log(chunk);
});
```

- **WRITESTREAM**
- Write it to a file
- ex.

```javascript
const writeStream = fs.createWriteStream("./docs/blog4.txt");
readStream.on("data", (chunk) => {
  // everytime we get a new chunk write it to a new file blog4.txt
  writeStream.write("\nNEW CHUNK\n");
  writeStream.write(chunk);
});
```

- **PIPING**
- Does the same thing but needs to be from a readStream to a writeStream

```javascript
readStream.pipe(writeStream);
```

# Client & Servers

- Client and server use the `HTTP`(Hyper Text Transfer Protocol) construct to communicate with each other.

```javascript
const http = require("http");

// create server
const server = http.createServer((req, res) => {
  // whenever server is called this will run
  console.log("request made");
});

// listen to server requests on port 3000
// localhost connects to your own computer ip address (127.0.0.1)
// when we go to localhost:3000 it will log it out
server.listen(3000, "localhost", () => {
  console.log("listening for requests on port 3000");
});
```

# Requests & Responses

## Request Object

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  // req object contains
  // headers of the request
  // url
  // expected response types
  // etc.
  // console.log(req);
  console.log(req.url); // if on localhost will give '/'
  // will give whatever after localhost
  // can be useful to implement routing
  console.log(req.method); // GET
});

server.listen(3000, "localhost", () => {
  console.log("listening for requests on port 3000");
});
```

## Response Object

- Sending plain text as response

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  // set header content type for the reponse
  res.setHeader("Content-type", "text/plain");
  // write what response we want to send back
  res.write("hello there");
  // end the response
  res.end();
});

server.listen(3000, "localhost", () => {
  console.log("listening for requests on port 3000");
});
```

- This is the response recieved by the browser
  ![](diagrams/responseobject.png)
