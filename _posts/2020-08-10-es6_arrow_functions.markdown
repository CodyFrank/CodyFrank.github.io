---
layout: post
title:      "ES6 Arrow functions"
date:       2020-08-10 20:41:56 +0000
permalink:  es6_arrow_functions
---


![](https://i.morioh.com/200713/73e1839f.jpg)

in my last blog post I talked about ES6 variables specifically why and how to use let and const and what problem they solve. This post is going to continue on the ES6 train and talk about arrow functions, why they are my favorite part about ES6 updates, and how / why to use them.

![](https://media.giphy.com/media/hujejOtss7zG0/giphy.gif)

## What are arrow functions
Simply put arrow functions are un named JavaScript functions. They have a much shorter syntax making them faster to write. They look like this...

```
() => {
console.log("Hello World")
}
```

Visibly they are not much shorter then the traditional function that looks like this ...

```
function() {
console.log("Hello World")
}
```

I mean come on you only dropped one word. Us programmers are lazy its our thing we want better.

In addition to a shorter syntax arrow functions have a very distint way to handle this but more on this later. First I would like to represent the full syntax of arrow functions. 

## Types of arrow functions
Arrow functions can be used without a name as shown above or you can sign them to a variable/constant and call them the same way a normal function would be called. 

*old*
```
function myFunction(args) {
  console.log(args)
}
```

***Written with arrow functions***
```
const myFunctions = (args) => {
  console.log(args)
}
```

Arrow functions could be shortend down to just one line without curly braces
```
const myFunction = (args) => console.log(args)
```

If the function *only has one argment* they can be even shorter by removing the braces around the arguments
```
const myFunction = args => console.log(args)
```

Now that looks like it is saving some space and typing.

## The difference between one line and multi line arrow functions.
They return keyword acts differently in arrow functions. In a multi line arrow function you must explicitly use the return keyword. Example...

```
const add = (a, b) => {
  return a + b
}
```

However in a one line arrow function this can be shortened yet again example...

```
const add = (a, b) => a + b
```

This will implicitly return the one line that is the arrow function. 

## When to use an arrow function. 
Arrow functions can be used in almost any use case however the shorter syntax can decrease readability so be careful. Arrow functions shine when used as an argument for another function called callback functions. since they are shorter and can be reduced to one line this spot is perfect for them

example...

```
const array = [ 0, 1, 2, 3, 4, 5 ]

// the filter method creates a new array with the elements that pass the test given as a callback function

const newArray = array.filter( num => num < 3 ) // returns [ 0, 1, 2, ]
```

## This in arrow functions.
This in a normal function (not arrow function) refers to whatever *calls* the function. that means the same function can have 2 different representations of this depending on how and what it is called by. In the arrow function this always refers to the object that defined the arrow function.

```
const obj = {
    id: 7,
    counter: function counter() {
      setTimeout(function() {
        console.log(this.id);
      }, 1000)
    }
  }

const obj2 = {
    id: 7,
    counter: function counter() {
        setTimeout(() => console.log(this.id), 1000)
    }
}
obj.counter()   // logs undefined
obj2.counter() // logs 7
```

The original function could solve this problem by calling the bind() method on the callback function but that makes for even more work. 

## Conclusion
Arrow functions were a confusing topic when trying to learn JavaScript for me. It felt like learning two languages. I hope this makes it simple for anyone who is learning JavaScript, learning ES6 syntax, or maybe learning to work with legacy code. 
