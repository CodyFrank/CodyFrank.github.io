---
layout: post
title:      "Sinatra project mode Pokemon"
date:       2019-08-22 02:00:07 +0000
permalink:  sinatra_project_mode_pokemon
---


### At last the long awaited Sinatra project has arrived

Over the last 7 or so weeks I have taken a dive into the world of Sinatra web development. This feels very rewarding as I have spent the prior 8 months dedicating my life to writing code and still have no idea how to get out of the terminal. Finally no more "puts"ing everything out.

## The Objective
* Build an MVC Sinatra application.
* Use ActiveRecord with Sinatra.
* Use multiple models.
* Use at least one has_many relationship on a User model and one belongs_to relationship on another model.
* Must have user accounts - users must be able to sign up, sign in, and sign out.
* Validate uniqueness of user login attribute (username or email).
* Once logged in, a user must have the ability to create, read, update and destroy the resource that belongs_to user.
* Ensure that users can edit and delete only their own resources - not resources created by other users.
* Validate user input so bad data cannot be persisted to the database.
* BONUS: Display validation failures to user with error messages. (This is an optional feature, challenge yourself and give it a shot!)

## Build an MVC Sinatra application.
This requirement felt very trivial and obvious as I spent the last 7 weeks in **Flatiron** learning how to use Sinatra and the MVC (Model View Controller) architecture style comes very easy to me. The MVC points to a way of structuring a file tree (and what is in those files) into 3 main catagories. 

#### Models 

This is where your classes are defined and where the logic in your application lives. My project has two models, Trainers and Pokemon. The model files include class definitions, has_many/belongs too relationship, and user validations.
```
class Trainer < ActiveRecord::Base

    validates_presence_of :email, :trainer_name
    validates_uniqueness_of :email, :trainer_name
    has_secure_password
    validates :password, length: { minimum: 5 }, confirmation: true, 
      unless: Proc.new { |t| t.password.blank? }
    has_many :pokemon
end
```
These classes inherite from ActiveRecord::Base(see active record section) and that gives them access to the metaprograming seen in the class

#### Views

This is where the HTML(hypertext markup language) and CSS (cascading style sheets) live. These two file types work together with ERB (embedded Ruby) to display your content to the user. The HTML is responsible for the content. The CSS is responsible for making it look good. Finally the ERB is just Ruby code that lives in this file to give some interaction to what the user sees. I attempted to keep as much Ruby code out of the views as possible as it is not the best location for it howevery the occasional *if* statement is needed.

`<%if @failed%>`

`<% @trainer.errors.full_messages.each do |e|%>`

`<p class="failed"><%= e %></p>`

`<%end%>`

`<%end%>`

The code above was used in my project to show error messages in my signup page (see bonus section for more details). It is a combination of HTML and Ruby inside of and ERB file. my views are further broken down into three sections Trainer, Pokemon, and Sessions. These directories hold the files that relate to those models and sessions for loging in/out and signing up.

#### Controllers

Controllers are used much like they were in the cli project by bridging the gap from the views and models. The controller is responsible for routes 

`get '/' do`

`erb :index`

`end`

These routes direct the application based on what the user puts into the url. the root route '/' would display the index.erb view to the user when the route '/' is entered into the url. a user changes what the application shows them by manipulating the url.

## use ActiveRecord and Sinatra

This also seemed like a no brainer to me as described above ActiveRecord was used in the models to give access to many methods. The function of these methods (and ActiveRecord) revolve around manipulating a database. In this case a SQLite3 Database. The ActiveRecord gem takes the task of writing SQLite3 and turns it into Ruby that **Flatiron** has been teaching us all about. Sinatra is the gem that is responsible for making the routes possible. Without Sinatra the application wouldn't have any url manipulation and without that a user would not be able to see any of the content or use the application at all.

```
    configure do
        set :views, 'app/views'
        set :public_folder, 'public'
        enable :sessions
        set :session_secret, SESSION_SECRET
    end
```

```
@pokemon = Pokemon.find_by_id(params[:id])
```
```
@pokemon.trainer.id
```

## Use multiple models and Use at least one has_many relationship on a User model and one belongs_to relationship on another model
This was shown in the MCV section as well where I used a Trainer model and Pokemon model. Trainers are like users where they have many pokemon and the Trainer info is used for signing in and out. Pokemon are the belongs too part of the relationship where a Trainer collects Pokemon.

## Must have user accounts - users must be able to sign up, sign in, and sign out. 
This is where the Sessions Comes in 
```
        enable :sessions
        set :session_secret, SESSION_SECRET
```

Sessions are Store as cookies that hold info about the user in the users browser. These cookies get sent back and forth on every request and continually validate the user allowing them to *be logged in*. This is done by turning sessions on and manipulating the sessions hash (seen below) to allow the application to persist state (or log in and out)
```
session[:user_id] = @trainer.id
```  

 ## Validate uniqueness of user login attribute (username or email).
 This was mostly completed with metaprogramming 
 `validates_uniqueness_of :email, :trainer_name`
 this seems like sudo code but it is actually ActiveRecord building a valid? method that I use explicitly `@trainer.valid?`
 in if statements to build errors and implicitly in the .save method `@trainer.save` the save method calls valid? method and will only save if valid? returns true.
 
 ## Once logged in, a user must have the ability to create, read, update and destroy the resource that belongs_to user.
 The resoure that belongs_to the user is the Pokemon. In my project I used RESTful routes to to perform the CRUD actions. 
 
 * Create

```
    pokemon = Pokemon.create(name: params[:name], nickname: params[:nickname], element: params[:element])
    if pokemon.valid?
        current_user.pokemon << pokemon
        redirect "/trainers/#{current_user.id}"
    else
        @messages = "There was a problem catching your pokemon. Please make sure all of the boxes are filled in"
        erb :'pokemon/new'
    end
```

* Read

```
    if @pokemon = Pokemon.find_by_id(params[:id])
      erb :'pokemon/show'
    else
      @messages = "oops! looks like that pokemon doesn't exist please try again."
      erb :'error'
    end
```

* Update

```
 if @pokemon = Pokemon.find_by_id(params[:id])
      if @pokemon.trainer.id == current_user.id
  
          if params[:name] != ""
            @pokemon.name = params[:name]
          end
  
          if params[:nickname] != ""
            @pokemon.nickname = params[:nickname]
          end
  
          if params[:element] != ""
            @pokemon.element = params[:element]
          end
  
          if @pokemon.valid?
            @pokemon.save
            redirect "pokemon/#{@pokemon.id}"
          else
            @messages = "Something has gone wrong"
            erb :'pokemon/edit'
          end
      else
          @messages = "That is not your Pokemon to edit"
          erb :error
      end
    else 
      @messages = "oops! looks like that pokemon doesn't exist please try again."
      erb :error
    end
```

* Destroy

```
    if @pokemon = Pokemon.find_by_id(params[:id])
      if @pokemon.trainer.id == current_user.id
          @pokemon.delete
          redirect '/pokemon'
      else 
          redirect "/pokemon/#{@pokemon.id}"
      end
    else
      @messages - "oops! looks like that pokemon doesn't exist please try again."
      erb :error
    end
  end
```

This is the main User interaction with Pokemon and what my project was based around.
all of these routes start with the authenticate helper method. This method uses another helper method logged_in? to only allow a logged in user to perform CRUD.

```
      def logged_in?
        !!session[:user_id]
      end
```

```
      def authenticate
        if !logged_in?
          redirect '/login'
        end
      end
```

## Ensure that users can edit and delete only their own resources - not resources created by other users.

This requirement is mostly completed with the current_user helper

```
      def current_user
        Trainer.find_by_id(session[:user_id])
      end
```

verifiying the the Pokemon's trainer is the same as the current user like so

```
    if @pokemon = Pokemon.find_by_id(params[:id])
      if @pokemon.trainer.id == current_user.id
```

this way if the current user does not own that object the user cannot manipulate it.

## Validate user input so bad data cannot be persisted to the database.
This requirement I found very difficult. In the end here is what I came up with.

```
      def clean(params)
        new_params = params.dup
        params.each do |k, v|
          new_params[k] = Rack::Utils.escape_html(v)
        end
        return new_params
      end
```

A helper method called clean that uses Rack::Utils method escape_html to sanitize the data a user puts into the program. This prevents the user from entering bad data and messing with my project. It works by taking an argument of a hash. We then pass the params hash (the users input data) into our helper method. This creates a copy of the hash that has been sanitized that gets used on our program. The routes look like this.

```
  clean_params = clean(params)
    pokemon = Pokemon.create(name:clean_params[:name], nickname: clean_params[:nickname], element: clean_params[:element])
```



## BONUS: Display validation failures to user with error messages. 

This is were the ERB really came into play. Inside my routes I set an instance variable @failed equal to false. if the validation failed the @failed would then be changed to true. It looks like this.

```
      post '/login' do
        @trainer = Trainer.find_by_trainer_name(params[:trainer_name])
        if !!@trainer && @trainer.authenticate(params[:password])
          session[:user_id] = @trainer.id
          redirect "/trainers"
        else
          @failed = true
          erb :'sessions/login'
        end
```

with the @failed declaired in the get request route. Then in the ERB view file I put an if statement saying if @failed is true then show error message. That looks like this.

```
<%if @failed%>
<p class="failed">Incorrect Trainer Name or Password</p>
<%end%>
```

The message in this case is always the same. That gives a potential Hacker the least amount of info as to make it more difficult for them. However the signup method has route set up the same way but uses the error messages built into the vaild? method i mentioned earlier. This allows the error messages to be more descriptive when signing up for easier user interaction. it looks like this.

```
<%if @failed%>
  <% @trainer.errors.full_messages.each do |e|%>
    <p class="failed"><%= e %></p>
  <%end%>
<%end%>
```

## conclusion
I have now completed my Sinatra Project and feel as I have learned far more in two weeks of a project then the past two months of coding. I am very thankful for the strict requirments on this project for without them I would have never pressed myself to get to the next level.

