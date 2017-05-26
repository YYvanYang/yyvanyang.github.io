---
layout: post
title:  "Understanding module.exports and exports in Node.js"
categories: node
---

As developers, we often face situations where we need to use unfamiliar code. A question will arise during these moments. How much time should I invest in understanding the code that I’m about to use? A typical answer is learn enough to start coding; then explore that topic further when time permits. 

## Defining a Node module

```javascript
// currency.js
const canadianDollar = 0.91;

function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

exports.canadianToUS = (canadian) => {
  return roundTwoDecimals(canadian * canadianDollar);
}

exports.USToCanadian = (us) => {
  return roundTwoDecimals(us / canadianDollar);
}
```

Note that only two properties of the exports object are set. This means only the two functions, canadianToUS and USToCanadian, can be accessed by the application including the module. The variable canadianDollar acts as a private variable that affects the logic in canadianToUS and USToCanadian but can’t be directly accessed by the application.


```javascript
// test-currency.js
const currency = require('./currency');
console.log('50 Canadian dollars equals this amount of US dollars:');
console.log(currency.canadianToUS(50));
console.log('30 US dollars equals this amount of anadian dollars:');
console.log(currency.USToCanadian(30));
```

## Important Points

The module.exports mechanism enables you to export a single variable, function, or object. If you create a module that populates both exports and module.exports, module.exports will be returned and exports will be ignored.

```javascript
// greetings.js

exports.sayHelloInEnglish = function() {
  return "HELLO";
};

exports.sayHelloInSpanish = function() {
  return "Hola";
};

/* 
 * this line of code re-assigns  
 * module.exports
 */
module.exports = "Bonjour";
```

Now let’s require greetings.js in main.js:

```javascript 

// main.js
var greetings = require("./greetings.js");

/*
 * TypeError: object Bonjour has no 
 * method 'sayHelloInEnglish'
 */
greetings.sayHelloInEnglish();
        
/* 
 * TypeError: object Bonjour has no 
 * method 'sayHelloInSpanish'
 */
greetings.sayHelloInSpanish();

// "Bonjour"
console.log(greetings);

```