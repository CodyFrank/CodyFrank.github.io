---
layout: post
title:      "What the heck is Node.JS???"
date:       2020-06-16 13:40:38 +0000
permalink:  what_the_heck_is_node_js
---


If the title is not a dead give away today Im going to be talking about Node.js. I have been exploring how it works and practicing with it for a few days now and I want to bring some light on this subject. I have notice that node is a tool that I deffinatly overlooked and gets washed away in other technologys, however It is vital to alot of these technologys being able to run. Lets take a look.

## What is it?
Node.js is a runtime for JavaScript(JS). Previously or without using node JS could only be exicuted in the browser. This made the JS language exclusivly client side or frontend. Node is simply a way to let a local machine read and run JavaScript. Node ***is not*** a programming language or a JavaScript Framework. 

## Background info (if you care to read the facts)
Node.js is an open source project created by Ryan Dahl (a programmer for joyent) in 2009. It is not the first tool to allow JavaScript to be ran on the server side but it is responsible for bringing the popularity to backend JS programming. When it first released it was only usable on Mac and Linux operating systems.  

## Why use Node?
Today in 2020 there are tons of programming languages and most of them do server side programming very efficently. Even in 2009 there were still more then enough other languages so why would someone take the time to bother creating Node? Why should anyone learn Node? Why do companies use Node? The first huge reason is beacuse it runs JavaScript. This means no more do web developers need to have different languages for server side and client side. Projects can be made much simpler and easier to create this way. Second JavaScript is practically the only single threaded asynchronus language. Node allows us to bring this technology into our backend. Node is event driven and features a non blocking I/O model making it very fast. Finally its popularity in the industry makes it a good tool for any developer to learn.

## Where does Node shine and where does it wall short?
Node is best suited for projects that are not cpu intensive. 

### Great for
* I/O operations - can be handle asynchronusly 
* Real time services like live updates and chats systems
* CRUD apps and rest api

### Not great for
* Sorting / Searching
* Complicated Algorithms
* large loops - loops cannot be avoided in programming but keep them as small as possible 

## How does it work?
Node is built on a module system. Its used by requiring (sometimes called importing) and exporting these modules. Node comes with many built in modules and a user can create there own by creating a new file. 
```
const fs = require('fs')
```

This code will declare a constent and and points to the fs or file system module. File system is one of the built in modules from Node.

When you create your own module you can export individual funtions like this

```
module.exports.sayName = sayName

module.exports.sayAddress = sayAddress
```

these can then be required in another folder like this

```
const sayName = require('./sayName')
```

please note the modules that are not built into node you must referance the file path name.

This and many other things node can do is made possible because all of these modules are wrapped in an invisible function. if it were written it would look like this.

```
(function (exports, require, module, __filename, __dirname){
 // module code lives here
 })
```

This wrapper is the way your files connect to the node modules and how node becomes possible. 

## What can Node.js do?
Well first of all you can access that wrapper function (shown above). This very easily gives us access to export and require modules or files. and gives us access to filename and dirname via variables. The rest of the functionality comes from these built in modules. I will list only a few with a short description. Please note the full listing can be found on the Node.JS documentation here https://nodejs.org/en/docs/.

1. os - lets us access operating system methods 
  example `os.homedir()`  can be used to find your machines home directory.
 
2. path - lets us access and operate on variuos file paths 
  example `path.join(__dirname, 'test', 'hello.html')`  returns a path in an order we specified. note join is used to concatenate
	
3. url - lets us operate on urls
  example `const myUrl = new URL('http://mywebsite.com/hello.html?id=100&status=active')`
	
4. fs - Lets us access the file system create read and write to files
  example: 
	
```
	fs.writeFile(path.join(__dirname, 'test', 'hello.txt'), "Hello World", err => {
     if (err) throw err
     console.log('file written too')
     fs.appendFile(path.join(__dirname, 'test', 'hello.txt'), " from node.js", err => {
         if (err) throw err
         console.log('file appended')
     } ) 
 })
```

5. event - lets us create event emitters and event listeners 
  example
	
```
	const EventEmitter = require('events')

// create emitter class

class MyEmitter extends EventEmitter {

}

//  init object

const myEmitter = new MyEmitter()

// create event listener

myEmitter.on('event', () => {
    console.log('event fired')
})

// 

myEmitter.emit('event')
``` 

6. http - my most used module. http allows us to create and access servers listen to servers for requests and responses. 

for example
```
const http = require('http')

server = http.createServer((req,res) => {
    res.write("hello World")
    res.end()
})
server.listen(3000, () => console.log("listening to port 3000"))
```

This is a very simple web server but this creates and listens to a server and sends hello world to the client side. 

When these modules are put together the posibilities are limitless. A few very simple examples would look like these.

```
const EventEmitter = require('events')
const uuid = require('uuid')


class Logger extends EventEmitter {
    log(msg) {
        // call event
        this.emit('message', {id: uuid.v4(), msg } )
    }
}


const fs = require('fs')
const path = require('path')


const logLocation = path.join(__dirname, 'logs.txt')

const logger = new Logger()

const time = new Date()


logger.on('message', (data) => {
    // console.log(`called listener`, data)
    fs.appendFile(logLocation, 
        `${time.toString()} ${data.id} ${data.msg}\n`, err => {
            if (err) throw err
            console.log("data logged to logs")
        })

})

logger.log("hello World")
logger.log("another log")
``` 

This one creates an event emitter and listener to log some data to another file.

or 

```
const server = http.createServer((req, res) => {
if(req.url === '/'){

fs.readFile(path.join(__dirname, 'index.html'), (err, data) => {
if(err) throw err
res.writeHead(200, { "Content-Type": "text/html"})
res.write(data)
res.end()
})

}
})

server.listen(3000, console.log("server is running"))
```

This one creates a server that listens for a http request then when the root page is accessed it reads an html file and sends that data to the client to be displayed. 

A slightly more advanced example of a server might look like this.

```
 let filePath = path.join(__dirname, 'public', req.url === '/' ? 'index.html' : req.url)
    let extName = path.extname(filePath)
    let contentType = 'text/html'
    switch(extName){
        case '.js':
            contentType = 'text/javascript'
            break
        case '.css':
            contentType = 'text/css'
            break
        case '.json':
            contentType = 'application/json'
            break
        case '.png':
            contentType = 'image/png'
            break
        case '.jpg':
            contentType = 'image/jpg'
            break
    }

    fs.readFile(filePath, (err, content) => {
        if (err) {
            if (err.code === 'ENOENT'){
                fs.readFile(path.join(__dirname, 'public', '404.html'), (err, content) => {
                    res.writeHead(200, { 'contentType': 'text.html' })
                    res.end(content, 'udf8')
                })
            } else {
                res.writeHead(500),
                res.end(`Server Error: ${err.code}`)
            } 
        } else {
            res.writeHead(200, { 'content-type': contentType })
            res.end(content, 'utf8')
        }

    })


})

const PORT = process.env.PORT || 3000
server.listen(PORT, () => console.log(`Server is running... port: ${PORT}`))
```

now I challenge you to spend some time practicing with Node.JS.
