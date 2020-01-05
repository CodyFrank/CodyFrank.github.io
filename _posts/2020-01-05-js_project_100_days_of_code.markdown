---
layout: post
title:      "JS project 100 Days Of Code"
date:       2020-01-05 18:46:44 -0500
permalink:  js_project_100_days_of_code
---


Weeks in the making I am finally finished with my Javascript Project. In FlatIron school we have spent 7 weeks Learning the basics to The javascript language and it has been an adventure to say the least. Im going to write a quick post talking about my project and the journey that has been javascript.

### Project Objective
* Design and architect features across frontend and backend
* Integrate JavaScript and Rails
* Debug issues in small- to medium-sized projects
* Build and iterate on a project MVP
In short this means you must have an application that uses a Rails backend api and a Javascript frontend that talk to each other. With that the possibilities are endless

## Requirements
* The application must be an HTML, CSS, and JavaScript frontend with a Rails API backend. All interactions between the client and the server must be handled asynchronously (AJAX) and use JSON as the communication format.

* The JavaScript application must use Object Oriented JavaScript (classes) to encapsulate related data and behavior.

* The domain model served by the Rails backend must include a resource with at least one has-many relationship.

* The backend and frontend must collaborate to demonstrate Client-Server Communication. Your application should have at least 3 AJAX calls, covering at least 2 of Create, Read, Update, and Delete (CRUD). Your client-side JavaScript code must use fetch with the appropriate HTTP verb, and your Rails API should use RESTful conventions.

## My Project
My project is a coding journal created to encourage people to write code and solve problems everyday. You can create Days that are like journal pages and challenges that are like journal entries.

## The application must be an HTML, CSS, and JavaScript frontend with a Rails API backend. All interactions between the client and the server must be handled asynchronously (AJAX) and use JSON as the communication format.
This is a very easily hit objective. When Broken down It basically means use HTML, CSS, JavaScript, and Rails to create a project. The only restriction is that your JavaScript frontend must talk to your Rails backend using AJAX and JSON. In my project I was able to complete this by making all of my interactions Fetch requests. 
*A side note on Fetch*  Fetch is a JavaScript function that abstracts the complexities of ajax away for you.
I setup adapter classes named DaysAdapter and ChallengesAdapter that are responsible for handling all my fetch requests. *more on this in the object oriented JavaScript section of this blog post*

```
class DaysAdapter {

	constructor(){
			this.baseUrl = "http://localhost:3000/api/v1/days"
	}
	}
	```
	
This is an example of one of my adapter classes. Notice I instantiate the objects with a baseUrl that matches the default url for my backend. This simplifies my methods later. One of those methods would be where the fetch request is performed and would look like this.
	
```
    async getDays() {
        return fetch(this.baseUrl).then(res => res.json())
				```
 
I used the async function because my method will return a promise or in other words its an asynchronous function. *a promise is a javascript object represent an imcomplete "but will be completed" operation* .then is used to wait for the promise to be completed then perform a callback function.

## The JavaScript application must use Object Oriented JavaScript (classes) to encapsulate related data and behavior.
I touched on this earlier in the blog but I chose to use the es6 class sintax for my project. It is most similar to Ruby and Is also the newest. Constructer is a method that is called when the object is created or instantiated with the new ClassName command 

``` new Challenge(c) ```

In this case c represents a string of json that javascript can interperate as an object. my challenge class taks this json and turns it into an object in the consctuctor method.

```
class Challenge{
    constructor(challengeJSON){
        this.id = challengeJSON.id
        this.dayId = challengeJSON.day_id
        this.question = challengeJSON.question
        this.description = challengeJSON.description
        this.solution = challengeJSON.solution
    }
}
``` 

In many of my classes I wrote more methods to be called on the objects. This is an example of that.

```    fetchAndLoadDays(){
        this.dayAdapter.getDays()
        .then(days => {
            days.forEach(day => this.days.push(new Day(day)))
        })
        .then(() => this.boundRender())
    }
```

## The domain model served by the Rails backend must include a resource with at least one has-many relationship.
This was covered in much more extent in the last blog post so I will simplify the rails portion of this section. In my rails models I used a has many / belongs too relation.

```
class Day < ApplicationRecord
    has_many :challenges
end
```

Then in JavaScript I made my Day class construtor method include an array of Challnge objects. and puch any already made challenges to the array

```
class Day{
    constructor(dayJSON){
		        this.challenges = []
        dayJSON.challenges.forEach(c => this.challenges.push(new Challenge(c)))
				  }
				}
				```
				
				In my challenge class I did the opposite to store a day id that referances the day objects id.
				
## The backend and frontend must collaborate to demonstrate Client-Server Communication. Your application should have at least 3 AJAX calls, covering at least 2 of Create, Read, Update, and Delete (CRUD). Your client-side JavaScript code must use fetch with the appropriate HTTP verb, and your Rails API should use RESTful conventions.
How this was done was explained earlier in the post. I should mention I did include full CRUD of day class and full crud of challenge class however I dont directly render (read) any challenges. They are all rendered by assosiation in my day controller. Between both DaysAdapter class and ChallengesAdapter class I have in total of 9 fetch request methods all the get called at some point in the application. I followed the appropriate HTTP verbs in my fetch request by added anything needed *like id* to the end of the base url that the adapter classes are created with. 

```
this.baseUrl = "http://localhost:3000/api/v1/challenges"
```
```
 return fetch(`${this.baseUrl}/${id}`
 ```
 
 to make
 ```
 "http://localhost:3000/api/v1/challenges/id"
```when needed.

I used restful actions and resources shortcut for restful conventions in my rails backend.

## conclusion
After alot of work and research I was able to put together a project that met the requirements. 
* Using HTML, CSS, JavaScript, and Rails
* ES6 class syntax for OOJS
* Rails has many belongs too relationships
* Finally including the appropriate Fetch Requests and engough of them.


