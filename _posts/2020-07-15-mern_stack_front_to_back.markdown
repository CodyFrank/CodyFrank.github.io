---
layout: post
title:      "MERN stack... Front to Back"
date:       2020-07-15 18:32:47 +0000
permalink:  mern_stack_front_to_back
---


![](https://img-a.udemycdn.com/course/750x422/2564962_073e_3.jpg)

** If you have been following my blog you may have noticed I made a series of posts talking about Databases, Express.js, Node.js, and (awhile back React.js). These blog post break down many of the technologies used to make up the MERN stack. This blog post is going to tie them all together. This post is intended to give a very basic understanding of MERN stack builds and a good grasp of what it is and why it is used. **

without further ado

![](https://media.giphy.com/media/W5UmXOcOd4ppwGHfqH/giphy.gif)

## What is the MERN stack?
MERN stack is a Full Stack (front-end and back-end, server side and client Side) group of tools that are used to build web applications. By tools web developers are refering to technologies(frameworks, databases, libraries, runtimes, ect). The MERN stack is built up of four main technologies. 

1. **M** is for MongoDB, a [non-relational (NoSQL)](http://https://codyfrank.github.io/non-relational_databases_nosql_databases) database used to store information. 
2. **E** is for Express.js ([express](https://codyfrank.github.io/expressway_to_a_backend)). This is a framwork that builds ontop of Node.js to create a back-end *(aka server-side)* application quickly and effeciently. 
3. **R** is for [React](https://codyfrank.github.io/react_is_it_javascript) a framework used to create front-end *(aka client-side)* applications.
4. **N** is for Node.js [(Node)](https://codyfrank.github.io/what_the_heck_is_node_js) a runtime ***not a framework*** that allows JavaScript to be interpreted on a machine rather then the browser. 

## Why use the MERN stack?
MERN is a very popular stack for web development meaning there are alot of job oppritunities using this stack.

**But why are so many people using it?**

![](https://media.giphy.com/media/s239QJIh56sRW/giphy.gif)

In todays web development scene it is almost a neccesity to learn some form of JavaScript. MERN stack uses only one language JavaScript meaning projects can no longer need to switch back and forth between programming languages. Developers also do not need to figure out how to get the 2 (or more) languages to talk to each other.

MERN stack is built to quickly and easily create web applications so whether your creating a personal project, a freelancer, or working for a major company everyone can appriciate less work and faster results.

## What should I know before jumping into MERN stack development?
1. JavaScript basics are a must. Working with variable's and data types, strong knowledge of functions, and syntax, as well as fetch or a similar tool (axios for example) Remeber the stack uses nothing but JavaScript so to really use it efficiently it is essential to have a very strong understanding of the fundementals before building apon them. Understanding and building with Express, Node, and React will all depend on your knowlegdge of the basics.
2. JavaScript ES6 syntax. Const and Let are very important to understand. As well as arrow functions, spread operator, destructuring, class syntax, ect.
3. Node.js including NPM at very least the basics. Express abstracts alot of the complexity of node away but I am a firm believer of knowing the "behind the scenes or under the hood" work. Without atleast basic knowledge of node it will not be possible to effeciently create a backend.
4. React.js many modern web applications are very "front end heavy" or have alot of the complexity handled in the client side. It is very important to have a very stong foundation of React to be able to create MERN apps. 
5. MongoDB similar to node alot of the complexity of Mongo is abstracted away with the use of an ORM, however an understanding of how to use and navigate MongoDB atlas, create projects and clusters, connect to a database, whitelist IP addresses, and a visualization of how the data looks when stored is crutial to being able to create apps. Any MERN stack app is going to have data that needs to be handled.
6. Express.JS this ties in quite a bit with node however starting a server, creating routes, handling middleware are all Vital to creating almost any app.
7. Basic knowledge of the internet including HTTP (requests,  responses, headers, statuses, error codes), REST.
8. JSON data 

As web applications grow in complexity and gain features there will be many other tools that will be required. Some examples of this include JSON Web Tokens (JWT) for authentication, Bootstrap and ReactStrap for UI design, CSS for UI design and styling, Heroku or other deployment services, ect.

## What does it Look like?

#### Setup
For starters make sure to have Node.js installed because this comes with NPM (what we will use for installing other dependancys). 

Create a new project folder somewhere on the computer. I have a dev folder and a projects folder inside that. When I start a new project I make a new folder in that projects folder named whatever my project is named. 

Run NPM init to create a package.json. 

Install any dependancies. (npm install < dependancy name > --< any flags >) I typically start with nodemon, concurrently, express, mongoose, and a few others depending on my features.

While dependancies are installing create a MongoDB atlas account or sign in. Create a new project and choose any needed cluster options and names. for most cases and smaller apps I use the free options. After the project and cluster is created create a user of that project. Typically under cluster > Database access > then add new database . *note this is not a MongoDB atlas user. Its for this project specifically*. The Final step to setting up Mongo is allowing needed IP addresses. In the cluster select network access and add IP address. Follow the prompts to add any needed IP addresses.

To create the front end React gives you a nice feature to create a whole folder for the client side. If react is globally installed Just type create react app < name of app or folder > --< any flags >. The structure of a MERN stack app and many modern web applications are 2 indiviule applications front and back that come together to create one full web application. 

#### Building the backend
*check out my previous blog posts about [Node.js](https://codyfrank.github.io/what_the_heck_is_node_js) and [Express.js](https://codyfrank.github.io/expressway_to_a_backend) for more info on building a backend*


Please note there are many things that can be done with Node.js, Express.js, and MongoDB, so every project will look different based on the needs and goals of the project. This is just a starting point and some examples.

I like to start with a file called server.js. Since Node is being use we must bring in the dependancies using the commenJS module system. Node is built on a module system as explained in my past blog. This commenJS looks like this. 
For more info on Node.js check out the [node documentation](https://nodejs.org/en/docs/)

```
const express = require('express')
const mongoose = require('mongoose')
```

The code above would bring express and mongoose into server.js file and save them as variables. *note Mongoose is an ORM for managing MongoDB.*

Next create your app by calling the express function just stored. 

```
const app = express()
```

after this ask express to listen to a port. In my case port 5000

```
// start listening to server
app.listen(5000, ()=>console.log(`server started`))
```

For more backend and server options check out the Express.js [documentation](https://expressjs.com/en/4x/api.html)

I store my MongoDB connection string in an enviornment variable to keep it secure. That makes a few lines of code to connect to mongo pretty reusable

```
// connect to mongo
mongoose.connect(process.env.MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true, useCreateIndex: true })
    .then(()=>console.log("connected to mongo"))
    .catch(err=>console.log(err))
```

The last bits that will be included in almost every MERN stack project server.js or like file is middleware. This can be used for many things but the very basic is routing and parsing JSON. I think of middleware as nothing more then a function that has access to the request, response, and next middleware function in the que. In the cases shown its used to parse data and direcgt routes but there are many more possiblities depending on each projects needs.

```
app.use(express.json())
```
This line here makes the data send to the backend readable. *side note this use to require a dependancy to parse. The most commen was body parser if you see and legacy code*

Routing
```
app.use('/api/items', require('./routes/api/items'))
```

This directs any requests made to the specified route, to the correct place. In this case it says any request made to /api/items should go to the route/api/items file (I keep my route here in this case).

The second file to setup in a MERN Stack is Models. This is where database configuration sometimes called schema is handled or created.

Sticking with the Items example I would create a folder from the project route called models and a file called Item.js
*Item singular and capitalized per convention*

Bring in mongoose and create the schema with commen.js
```
const mongoose = require('mongoose')
const Schema = mongoose.Schema
```

Now simply tell the schema what it should look like

```
const ItemSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    date: {
        type: Date,
        default: Date.now
    }
})
```

The example above says we should have a collection or JSON object with two keys name and date those keys should expect a value that is either String for name and Date for the date. The name key value pair is required and the date key value pair will default to date now. There are many configurations that can be done here check some out at the mongoose [documentation](https://mongoosejs.com/docs/).

last thing for this file is to export it so other files or modules can bring them in(to my routes files up next)

```
module.exports = Item = mongoose.model('item', ItemSchema)
```

The above line of code tells other modules to import this data useing Item in commenJS, and it tells MongoDB to call this collection Items.

The final part to a rest api backend would be what to do with these requests and responses. This is handled in the routes. I mentioned before this we directed the request in the server.js file to our route in a file routes/api/items.js

Start by bringing in express, router(from express), and the model(just created it) *more commenJS imports*

```
const express = require('express')
const router = express.Router()
const Item = require('../../models/Item')
```

Then we write a function to handle the different types of request. For example below will handle a GET request to /api/items, then get the Items from the database, sort them by date decending, and finally respond with the JSON data just retrieved from MongoDB.

```
router.get('/', (req, res) => {
    Item.find()
    .sort({ date: -1 })
        .then(items => res.json(items))
})
```

Routes can be created to handle many different requests and do many things from retrive data from the database, insert data to a database, delete data, update data, and more.

#### Building the frontend

As stated above I reccomend useing create react app to create a new folder that will hold all client side data. A package.json file is then created for the frontend application. ***BE CAREFUL TO ADD DEPENDANCYS TO THE CORRECT FOLDER*** and just in general be mindfull of what directory your terminal is currently in. It can be very easy to push the wrong data to github or try to start a server from the wrong place. 

For dependancys on the front end thhis can change alot depending on preferance and the goals of the project. some common ones are Redux, React-Redux, Reactstrap, Bootstrap, Axios, Redux-Thunk, ect.

Building the Front end application will change dramatically so I wont go in such detail. Please refer to the React [documentation](https://reactjs.org/docs/getting-started.html) for insight on how to build the front end or my [blog post](https://codyfrank.github.io/react_project_d_and_d_5e_character_sheet) on React for a referance to a past React project I built. 

#### Running the servers locally

When building the backend there is likely only one server to run. In terminal from the project root the command npm start will start the backend. I mentioned earlier two more packages Nodemon, and Concurrently. These are used for runnint the various servers. I like to create a few npm scripts for the differenst servers and use these in place. 

```
"server": "nodemon server.js",
"client": "npm start --prefix client",
"dev": "concurrently \"npm run server\" \"npm run client\"",
```

The first one server could be called in terminal npm run server. It is what I recommend using for backend development. It will start the server with nodemon which will update the server on a save so there is no more need to stop and restart the server after ever change in code. 

The second is client and should be ran with npm run client. This will start the frontend and launch it in your browser. React will handle starting the server on every save for you. 

The third is both backend server and front end server. This one uses Concurrently to run both the nodemon server and the client server at the same time. This allows for much easier development and debugging. 

## Conclusion
MERN stack is a very fast and powerful web development stack that is widly used. It is very fun and well rounded for great for man situations. It is very clean and minimalist while still has a wide range of possibilities.
