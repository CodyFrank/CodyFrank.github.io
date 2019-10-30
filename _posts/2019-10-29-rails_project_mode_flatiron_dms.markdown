---
layout: post
title:      "Rails Project Mode Flatiron DMS"
date:       2019-10-30 03:13:51 +0000
permalink:  rails_project_mode_flatiron_dms
---


If you have been following my blog you probably know I have been learning Ruby over the course of the past few months. If you haven't been then check out my blog!.

This post is really going to be an explaination of Ruby on Rails and as such Im not going to spend alot of time talking about things that Rails and Sinatra share.


## Routes.rb
One of the first things I notices when transitioning into rails was the controller is totally different. no longer are we defining route paths in out controllers Rails gives us a config/routes.rb file for them. The Mvc structure still remains of course but this changes the way out controllers look.

```
  Rails.application.routes.draw do
  root 'application#home'
  resources :customers, except: [:new]
  resources :employees do
    resources :repair_orders, only: [:show, :index]
  end
  resources :jobs, except: [:new, :edit, :index, :show]
  resources :repair_orders do
    resources :jobs, only: [:new, :edit]
  end
  resources :vehicles, except: [:show, :edit, :update]

  get '/signup' => 'customers#new', as: 'signup'

  get '/login' => 'sessions#new', as: 'login'
  post '/login' => 'sessions#create'
  delete '/logout' => 'sessions#destroy'

  get '/auth/facebook/callback' => 'sessions#facebook_create'
  get '/login/employee' => 'sessions#new_employee', as: 'login_employee'
  post '/login/employee' => 'sessions#create_employee'

end
```

note the resources just writes the get and post methods of the restful routes for us.
## Generators
Generators are a feature build into rails that can be used to write code and build a file tree much faster.
They are command line commands that create sections of code for us. They can be used for things as small as
* creating a blank migration(or filled in with the correct arguments)
to as large
* building a full Restful a scaffold with tests in once line.
For obviouse reasons this makes rails  **powerful** and *very fun*. 
In my project I used 
* rails generate resources (to create routes and controllers)
* rails g (short for generate) model (to create a model)
* rails g migration (many time to create and update the database)

## Validations
Validations in Rails are very similar to Sinatra However I did learn how to create custom validators using 
* validate method
* ActiveModel::Validator 
* creating our own errors.messages.
only validates was needed in my project
```
    validates :name, :worker_number, :job_title, presence: true
    validates :worker_number, uniqueness: true
    validates :password, confirmation: true
    validates :password_confirmation, presence: true, on: :create
    validates :worker_number, length: { is: 4 }
```

## Associations
Rails associations are also very similar to sinatra however now I was able to expand into has may through. This allowed me to really experiment with join tables and even create a repair orders join table that the user can imput on. I also used dependent destroy so when a repair order is deleted it also destroys jobs. This prevents me from having an every growing database with alot of info lost in space.
```
    has_many :repair_orders
    has_many :vehicles, through: :repair_orders
    has_many :jobs, through: :repair_orders
    has_many :customers, through: :repair_orders
    
```

```
    belongs_to :employee
    belongs_to :vehicle
    belongs_to :customer
    has_many :jobs, dependent: :destroy
```

## Action View
Action View is new to us Flatiron students in this section. It allows us to stop hard coding alot of our html and paths.
In the project the most commen use for it was for directing the url. 
`customers_path` references /customers and that would be the index page. Stepping up to the next level I used `customer_path(@customer` where an object of customer gets passed into the methed. this would take users to the customer show. I also defined my own action view methods in routes.rb
`get '/signup' => 'customers#new', as: 'signup'`  allows an action view method like `signup_path`

## Forms
I have 2 types of forms I used
1. form_tag
 ```
<%= form_tag("/login") do %>

  <div class="form_box">
    <%= label_tag :email %>
    <%= text_field_tag :email %>
  </div>

  <div class="form_box">
    <%= label_tag :password %>
    <%= password_field_tag :password %>
  </div>

  <%= submit_tag "Login" %>
<% end %>
```
Form tag generate some html for us and helps save some time
2. form_with (form_for)
form_for also generate html but it uses a form builder. This takes in an argument of an object and can send post request to /create or /update depending on if the object is saved. It allowed me to use one form for both new and edit actions just by passing in a saved or unsaved object.
<%= form_with model: employee, local: true do |f| %>

<div class="form_box">
  <%= f.label :name %>
  <%= f.text_field :name %>
</div>

<div class="form_box">
  <%= f.label :worker_number %>
  <%= f.text_field :worker_number %>
</div>

<div class="form_box">
  <%= f.label :job_title %>
  <%= f.text_field :job_title %>
</div>

<div class="form_box">
  <%= f.label :password %>
  <%= f.password_field :password %>
</div>

<div class="form_box">
  <%= f.label :password_confirmation %>
  <%= f.password_field :password_confirmation %>
</div>

  <%= f.label :admin %>
  <%= f.check_box :admin %>

  <%= f.submit %>

<% end %>

## Strong Params
Strong params has changed from Sinatra but still has a similar purpose. It protects Rails projects from bad data.
```
    def employee_params
        params.require(:employee).permit(:name, :worker_number, :job_title, :password, :password_confirmation, :admin)
    end
```
is how I told Rails what data is okay based on the params hash. Rails has a few checks build in to make sure the data wont hurt my project. Once this is done I can pass the method into a class.new or class.create to mass assign a new object from that class. `Employee.new(employee_params)`

## Scope Methods
New to me in the Rails sections Scope methods are a wait of querying data from the database. In my project I used 
```
scope :administrators, -> { where(admin: true) }
```
to give me a collection of employees who have admin checked true. These methods gain alot of power because they are chainable meaning I could make another scope method and call it on the administrators listed above to narrow my search.

## Omniauth
Omniauth is a gem I used to allow customers to log in with facebook. I works by sending the url to facebook having the guest sign in with them. A few checks later and facebook sends back the info requested.




