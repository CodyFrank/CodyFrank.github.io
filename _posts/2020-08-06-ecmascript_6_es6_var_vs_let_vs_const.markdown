---
layout: post
title:      "ECMAScript 6 (ES6 var vs. let vs. const)"
date:       2020-08-06 13:36:35 +0000
permalink:  ecmascript_6_es6_var_vs_let_vs_const
---


According to Stack Overflows 2019 developer survey JavaScript is the most popular language among developers for the 7th Year in a row. Many of these JavaScript developers learned the language in 2016 or later. This means there are lots of web developers who learned JavaScript after 2015 when ECMAScript 6 or ES6 released. In this blog post I hope to compair variable declarations. Hopefully this will help newer developers adapt to working with legacy code more easily and quickly. 

![](https://i.morioh.com/2020/03/21/4c2be688efe4.jpg)

## Why ECMAScript not JavaScript 6?
In 1995 Brendan Eich created Mocha (later JavaScript because Java was a popular programming language at the time) while working for a Netscape. Mocha was later changed to Livescript and eventually JavaScript. Due to competition from Microsoft and the release of many new browsers Netscape had to shut its project. Before shutting the project Netscape created a standard for JavaScript called ECMAScript. 


## Variable declaration var vs let vs const

#### var syntax
```
var a = "1"
```

var is the pre es6 way to declair variables. It is globally scoped or function scoped. When decalred outside of a variable var is globally scoped meaning the wholoe window has access to it. When declared in a function var is function scoped meaning only that function has acess to it. 

example
```
var hello = "Hey"

function sayHello(){
    var greeting = "Hi"
}

console.log(greeting) // Uncaught ReferenceError: greeting is not defined
```

var can be redeclared an updated for example

```
var a = "a"
var a = 1
a = "example"
```
This is all possible with the var variable declaration. 

Hoisting with var - declarations get hoisted to the top of their scope with var meaning this ...

```
console.log(hi)
var hi = "hello"
```
will get interpreted like this
```
var hi
console.log(hi) // hi is undefined
hi = "hello"
```

#### Why I never use var?
It can cause bugs that could be hard to find. For example
```
var i = 0
var num = 10

// lots of code here


if ( i === 0 ) {
    var num = 100
}
console.log(num)   // 100

if (num === 10){
console.log("really cool stuff is happening here")
} // really cool stuff never runs
```
Really cool stuff will not happen because num was unexpectedly changed to 100

lets talk about why using let and const solves this problem

## Variable declaration let & const
#### Scope
Both const and let are block scoped meaning they can only be accessed in the block they are defined ( inside the curly braces its defined )
example 
```
let greeting = "Hello"
function sayHi(){
    console.log(greeting") // logs "Hello"
		let hi = "hi"
}

console.log(hi) // hi is undefined
```

#### Redefine
Let can be redefined or updated however const cannot
example
```
const hi = "hello"
let greeting = "hey"

greeting = "greetings"
hi = "hi" // throws an error

console.log(greeting) // logs greetings
```
neither let nor const can be redeclared
example
```
let hi = "hi"
const hey = "hey"

let hi = "hello" // throws an error
const hey = "hello" // throws an error 
```

#### Let and Const hoisting
const and let are hoisted alittle differently then var

example this
```
console.log(hi)
let hi = "hi"
```
gets interpreted like this
```
let hi
console.log(hi)
hi = "hi"
```
however let is not initilized as undefined like var is meaning the above code will still throw an error.

#### How do these solve the problem with var?
letts bring back the example above and see what happens when we change it to let or const.
```
let i = 0
const num = 10

// lots of code here


if ( i === 0 ) {
    const num = 100 // throws an error rather then unexpected output
}
console.log(num)   // 100

if (num === 10){
console.log("really cool stuff is happening here")
} // really cool stuff never runs
```

Since const throws an error we can easily debug why really cool stuff didn't happen by reading the error, seeing that const cannot be reassigned, then changing the variable name to somthing different. leaving us with this working code...
```
let i = 0
const num = 10

// lots of code here


if ( i === 0 ) {
    const oneHundred = 100
}
console.log(num)   // 10

if (num === 10){
console.log("really cool stuff is happening here")
} // really cool stuff never runs
```

In this example num is not redefined, meaning num does === 10 and really cool stuff will happen as we expected.

#### A note about const 
const does allow for addition to arrays and objects. 
example.
```
const array = []
array.push(1)
console.log(array) // [1]
```

This means const is a good option for arrays and objects beacsue they are usually not likely to be updated and you can still operate on them.

## Conclusion
In general var can lead to unexpected bugs that are very difficult to track down and fix. To help resolve this issue, it is best practice to use const whenever possible (the variable will not change) and let whenever the variable changes. To avoid any hoisting issues it helps to alway declare and assign variables before they are used. 
