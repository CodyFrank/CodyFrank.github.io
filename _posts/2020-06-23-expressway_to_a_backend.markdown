---
layout: post
title:      "Expressway to a Backend"
date:       2020-06-23 14:13:37 -0400
permalink:  expressway_to_a_backend
---


If you follow my blog you may have read my talk about Node.JS and how awesome it is and how many problems it solves. If you havent check it out [here](https://codyfrank.github.io/what_the_heck_is_node_js). In there I talk about how Node is a runtime for JavaScript and how it can be used to create a backend or api. I even include and example on a REST api. 

## Node.js has a huge problem
Well Node.js has alot of huge problems according to Ryan Dahl the original creater of Node.js, check them out [here](https://www.youtube.com/watch?v=M3BM9TB-8yA), but these are not the problem im refering to. Im talking about this.
```
const http = require('http')
const path = require('path')
const fs = require('fs')
const server = http.createServer((req, res) => {
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

Who in the world wants to write all of this just to get a web server up and running. There are so many web frameworks out there now that can do this like a million times faster. 

## The Express Solution
This is where Express.js shines. Express.js or Express is a JavaScript Framework built ontop of Node that makes it feasible to use JavaScript as the language for your server side application. You could have a server up and running with as little as 5 lines of code. This snippet is pulled right from the express documentation.

```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`))
```

This code makes the most simple server and displays hello world on the screen. 

Express is not the only framework that can be used to build a JS backend but it is by far the most popular.

## Fast, Unopinionated, Minimalist, Web Framework for Node.js
*  #### Fast - 
Express is very powerful in that you can create a server with not alot of code. This makes it fast or quickly to get running.

* #### Unopinionated -
Express gives the engineer alot of freedom to write the code as they see fit. You are not bound by a lot of prexisting patterns to follow so developers can build what they need how they need it without worry of breaking somthing in the framework or extra coding to make it happen.

* #### Minimalist -
Express gives you a very bare minimum amount of code with the framework keeping applications simple. Without extra unneeded or unused code the programs become more readable, smaller, better looking, easier to scale and debug, and results in less work on the engineers. Gone are the days of sifting through unused files and unused lines of code cleaning and deleteing. Say goodbye to debugging code you didnt write trying to figure out what you deleted that was needed. Plus less things to mess up make it easier to learn.

These put together are the reasons why Express is so popular and why so many people love it so much.

## What does it look like in action?
Express works with node and adds extra funtionality so for starters you require or import code just like you would in Node.

### Server and Listening
```
const express = require('express')
const app = express()
```

then simply tell the app vaiable (that stores the express() function) to listen on a port number

```
const PORT = process.env.PORT || 3000
app.listen(PORT, ()=>console.log(`Server started on port ${PORT}`))
```

and you have a running server. please note that the second argument in the listen function provided by express is a callback. In the example a passed in a regular JavaScript arrow function (no need for express here) that logs to the console that my server is running and what port number. I used this for development so I can see what port I need to go to for localhost and know when my server is running. Second note I saved an enviornment variable PORT. This is simply incase I plan to deploy this app to Heroku later on.

### Routes
Next we need to define some routes. A pretty simple example would look somthing like this.

```
router.get('/', (req, res)=>{
    res.json(members)
})
```

First off I would like to mention that I used router. I imported this from express at the top of the module. router allows me to use module route handling. (more on this later but basically I have a seperate file to handle this paths route. router lets me do this). If I wasnt planning on alot of routes I could just do this in my index.js module / file and use app.get(...).

This snippet above is telling my app to respond to a get request (a type of http request). I could also use post, put, patch, delete. These http request types or methods have different functions in express but they take the same arguments. example.

```
router.post('/', (req, res)=>{
    member = {
        id: uuid.v4(),
        name: req.body.name,
        age: req.body.age
    }

    if (!member.name || !member.age) {
        return res.status(200).json({ msg: "please include a name and age"})
    }

    members.push(member)
    res.send(members)
    // res.redirect('/')
})
```

The first argument to these routing functions is a string that will represent the url that this method is responding to. The second is a callback funtion (vanilla JS again) that has access to the http request and the http response. The calback is how devs handle the route or what do you want to hapen when somone travels to this page. 

If any parameters were included in the route like this.

```
router.patch('/:id', (req, res) => {
    const found = members.some( member => member.id === parseInt(req.params.id))

    if (found) {
        const updMember = req.body
        members.forEach(member => {
            if (member.id === parseInt(req.params.id)) {
                member.name = updMember.name ? updMember.name : member.name
                member.age = updMember.age ? updMember.age : mamber.age
                res.json(member)
            }
        })
    } else {
        res.status(400).json({ msg: `no member with the id of ${req.params.id}`})
    }
})
```

They can be accessed through the req (request) argument. req.params.id as shown above or req.params.whatever parameter is named in the url argument. This would be commenly used to find a specific bit of data from the database. 

### Middleware
Middleware in express is a function that has access to the request and response parameters much like the routes. however middleware also has access to the next parameter. This next refers to the next function in the router. These functions can

* Execute any code 
* Make changes to the req and res objects
* End the request-response cycle
* Call the next middleware in the stack

these can be used for a veriety of things in my case I used it to direct my routes to a different file

```
app.use('/api/members', require('./routes/api/members'))
```

or use outside configuration 

```
app.use(express.json())
app.use(express.urlencoded({ extended: false }))
```

Some middle ware will require the next() parameter to be called or else it well never call the next peice of middleware.

ex. from the express documentation

```
var myLogger = function (req, res, next) {
  console.log('LOGGED')
  next()
}
```

### Views
Many express apps will be used to create an api that will not feature views. This is done by calling the json() method in the routes and passing in what you want to be sent as json. However if you require views express supports 3rd party template engines to render view files. The supported template engines can be found on the express documentaion page [here](https://expressjs.com/en/resources/template-engines.html). Some popular ones are Pug, Haml, Handlebars, and the default is Jade. 

```
app.engine('handlebars', exphbs({ defaultLayout: 'main'}))
app.set('view engine', 'handlebars')
```

The code listed above tells express to use handlebars rather then the default.

### Models
Express is commenly paired with a nosql database called mongoDB. This is done by using a package called Mongoose to connect to a mongo database / cluster. This blog post is not intended to show the inner workings of mongoose or mongo but I will breifly show some code used in one of my todo app projects that shows connecting to the database and createing the schema in the model.


creating the schema
```
const mongoose = require('mongoose')

const todoSchema = mongoose.Schema({
    title: {
        type: String,
        required: true
    },
    description: {
        type: String,
        required: true
    },
    date: {
        type: Date,
        default: Date.now
    }
})

module.exports = mongoose.model('todos', todoSchema)
```

Using the Todo model above in my routes

```
const Todo = require('../models/Todo')

// todo index
router.get('/', async (req, res) => {
    try{
        const todos = await Todo.find()
        res.json(todos)
    }catch(err){
        res.json({ message: err})
    }
})
```

connecting the Model to the mongo db database

```
mongoose.connect(process.env.DB_CONNECTION, 
    {
    useNewUrlParser: true,
    useUnifiedTopology: true 
    }, console.log("connected to db")
)
```

note for secureity reasons I used the dotenv package to store the connection string (includes my db password and username ect...) in a enviornment variable that does not get pushed to github.

### Generator

Express comes with a built in generator to create a scaffolding of your site. This can very quickly get a server up and running however is typically not used for large scale projects. The documentation shows the numerous options that can be passed to the generator. Basically this is a terminal command that can create files and write some basic code for you. Check out the docs [here](https://expressjs.com/en/starter/generator.html)


## Conclusion

Express is a very powerful web framework build on node. It allows us to build much larger project much more quickly then the base toolkit provided with Node. Using express in my personal opinion is very rewarding and fun to use.



