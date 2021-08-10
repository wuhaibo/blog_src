---
title: node.js export
date: 2021-08-10 16:47:41
updated: 2021-08-10 16:47:41
tags:
  - node.js
  - export
categories:
  - 技术
comments: true
---
The `module.exports` is a special object which is included in every JavaScript file in the Node.js application by default. The `module` is a variable that represents the current module, and `exports` is an object that will be exposed as a module. So, whatever you assign to `module.exports` will be exposed as a module.

### Export function

```javascript
// log.js

module.exports.debug = function(msg){
  console.log("[debug]: " + msg);
}

// app.js
var logger = require('./log')
log.debug("hello world")
```

### Export object

```javascript
// log.js

const logger = {}
logger.debug = function(msg){
  console.log("[debug]: " + msg);
}

module.exports = logger

// app.js
var logger = require('./log')
log.debug("hello world")
```

### Use only part of the exported object

```javascript
// log.js

const logger = {}
logger.debug = function(msg){
  console.log("[debug]: " + msg);
}

logger.error = function(msg)

module.exports = logger

// app.js
var { error } = require('./log')
error("hello world")
```

### If name confliction

```javascript
// log.js

const logger = {}
logger.debug = function(msg){
  console.log("[debug]: " + msg);
}

logger.error = function(msg)

module.exports = logger

// app.js
const { error: logError } = require("./hbUtils");

let error = "error here";

logError(error);
```
### Export class

```javascript
// student.js
class Student {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }
  getFullname = () => [this.firstname, this.lastname].join(" ");
}

module.exports = Student;


// app.js
const Student = require("./student");

const williamwood = new Student("william", "wood");
console.log(williamwood.getFullname());
``` 

### Export function as class

```javascript
// student.js
module.exports = function (firstname, lastname) {
  this.firstname = firstname;
  this.lastname = lastname;
  this.getFullname = () => [this.firstname, this.lastname].join(" ");
};


// app.js
const Student = require("./student");

const williamwood = new Student("william", "wood");
console.log(williamwood.getFullname());
```

