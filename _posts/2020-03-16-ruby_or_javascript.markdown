---
layout: post
title:      "Ruby or JavaScript"
date:       2020-03-17 00:35:25 +0000
permalink:  ruby_or_javascript
---


Over the last year I have been attending Flatiron Schools Software Engineering program. This program includes learning the basics of two very different programimg languages Ruby and JavaScript. While they are both popular and powerful languages, they have major differences. In this blog post I am going to compair the syntax of the two languages.

## Syntax
Syntax refers to the way the language is written. You can have 2 peices of code that do the same thing however when written can look very different. 

### Javascript
Brendan Eich summarized the ancestry of the syntax in the first paragraph of the JavaScript 1.1 specification[1] as follows:

JavaScript borrows most of its syntax from Java, but also inherits from Awk and Perl, with some indirect influence from Self in its object prototype system.

### Weakly vs. Strongly typed
JavaScript is a weakly typed programming language. This means that JavaScript will implicitly convert data types. For example the string "1" plus the integer 1 will return the string of 11

```
"1" + 1
"11"
```

This can make debugging alittle more difficult because the outcomes are not always as predictable. Mastering the arts of JavaScripts implicit conversions take alot of practice and to a beginner it can be very easy to forget about or lose a return value in your code. On the other hand JavaScript will do everything in its power to help you.

Ruby on the other hand is a very strongly typed language. This means Ruby will inspect the data's type eg "string", integer, ect before perfoming an operation. 
using the same example above Ruby will throw an error.

```
"1" + 1
Traceback (most recent call last):
        5: from /home/cody/.rvm/rubies/ruby-2.6.1/bin/irb:23:in `<main>'
        4: from /home/cody/.rvm/rubies/ruby-2.6.1/bin/irb:23:in `load'
        3: from /home/cody/.rvm/rubies/ruby-2.6.1/lib/ruby/gems/2.6.0/gems/irb-1.0.0/exe/irb:11:in `<top (required)>'
        2: from (irb):1
        1: from (irb):1:in `+'
TypeError (no implicit conversion of Integer into String)
```

This error will likely break your code making it stop execution completely. However you will not get any unexpected response and Ruby will give you as much data as it can to help you find and fix the problem.

### Syntactic Suger

Both JavaScript and Ruby have shortcuts in the syntax or way the code can be written. This sugar can make the code more understandable, easier to write, or just shorter. This can make the languages look vastly different even in the same language.

#### Javascript
* Arrow functions - these are unnamed functions that can be written inline and do not require the function declaration.
standard function
```
var add = function(x, y) { 
return x + y;
};
```
arrow function
```
let add = (x, y) => { return x + y };
```

Both of these functions do the same thing but look different while doing it. 

* Destructoring - in JavaScript you can pick exactly what elements we want out of a collection (array, object, ect) 
In the long form a programer would need to use the bracket notation or dot notation to select each element from the collection and assign them to their own variables. This would look like this

```
const person = {
    name: "Harry Potter", 
    age: 16, 
    wizard: true
}
const name = person.name

name
"Harry Potter"

const age = person.age

age
16
```

Using destructoring on the same object we could assign the same variables in a much faster way

```
const person = {
    name: "Harry Potter", 
    age: 16, 
    wizard: true
}

const { name, age } = person

name
"Harry Potter"

age
16


```

much less code to get the same variables.

#### Ruby

Ruby is designed for programmers satisfaction so alot of the syntactic suger is just built right into Ruby. It makes for a beautiful language. 

keyword arguments - this allows you to define methods with symbols and then call the methods with key value pairs. This removes some chance for error and sets the groud for mass assignment.

example
```
def hello_world(greeting: "Hello", planet: "World")
   puts "#{greeting}, #{planet}"
end
 => :hello_world
hello_world
  Hello, World
 => nil
 hello_world(greeting: "Hola", planet: "Mars")
 Hola, Mars
 => nil
```

then taking it one step further to mass assignment we can instantiate an object with a hash
```
class Wizard
  attr_accessor :name, :age
  def initialize(name:, age:)
     @name = name
     @age = age
   end
end

 wizard_attributes = { age: 16, name: "Harry Potter" }

 harry = Wizard.new(wizard_attributes)
 => #<Wizard:0x00007fffd7cd8758 @name="Harry Potter", @age=16>
harry.name
 => "Harry Potter"
```

