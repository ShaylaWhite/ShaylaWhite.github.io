---
layout: post
title:      "Weight Tracker"
date:       2020-05-14 23:31:57 -0400
permalink:  weight_tracker
---


Since the start of the quarantine with just eating and coding ALLLLLL day, I definitely put on my share fair of extra pounds

As it turns out so has my friends and family. As a friendly competition we decided to challenge and encourage each other to stay fit and as a result Weight-Tracker was born. *

***Weight-Tracker will allow users to:***

	| log in/log out/sign up
	| view/edit/add/deletes stats
	
	
	In order to do so users need:
	
	| authentication
	| emails/user only email
	| password
	| sessions
	
In order to achieve these goals, I implemented MVC, Sinatra Active Record CRUD, & Sessions.

# **Model:The Database **

*ENCAPSULATES THE DATA SPECIFIC TO OUR APPLICATON 
LOGIC AND COMPLUATAION AND PROCESS THE DATA*

Activerecord cooks up all of the database data by the use of migrations and models. 


***Users***

```
class User < ActiveRecord::Base 
  has_many :stats
end
```


```
class CreateUsers < ActiveRecord::Migration 
  def change 
    create_table :users do |t| 
      t.string :name
      t.string :email
      t.string :password 
    end 
  end 
end
```

**Stats**

```
class User < ActiveRecord::Base 
  belongs_to: user
end
```


```
class CreateStats < ActiveRecord::Migration 
  def change 
    create_table :blogs do |t| 
      t.string :weight
    end 
  end 
end
```


# **View**

What the user sees Is displayed by html and embdeed ruby. 

Forms displayed on the allows user to enter input.

This ia an example of a Form that allows the user to delete a stat.

```
<form action="/stats/<%= @stat.id %>" method="post">
  <input id="hidden" type="hidden" name="_method" value="delete">
  <input type="submit" value="delete">
</form>

```


# **Controller**

Intermediary for the Model and View.

The coontroller is able to commmunitcte between the model and view with the use of routes.
  
	
	These routes below will help the user create a new stat, edit, delete and update a stat
  # new
(The first one is a GET request to load the form to create a new Stat)
  ```
get '/stats/new' do
    erb :new
  end
```

(The second action is the create action. This action responds to a POST request and creates a new stat based on the params from the form and saves it to the database. Once the item is created, this action redirects to the show page)
```
post '/stats' do
  @stat = Stat.create(:weight => params[:weight],)
 redirect to /stats/#{@stat.id}
end 
```
  
In order to display a single Stat, we need a show action. This controller action responds to a GET request to the route '/articles/:id'. Because this route uses a dynamic URL, we can access the ID of the stat in the view through the params hash.
  # show
```
  get '/stats/:id' do
    @stat = Stat.find(params[:id])
    erb :show
  end
```





  # create
  ```
post '/stats' do
    @stat = Stat.create(params)
  redirect "/stats/#{@stat.id}"
  end
```
  
  # index
  ```
get '/stats' do
    @stats = Stat.all
    erb :index
  end
```
  
  # show
  ```
get '/stats/:id' do
    @stat = Stat.find(params[:id])
    erb :show
  end
```
  

  # edit 
 ```
#load edit form
  get '/stats/:id/edit' do
    @stat = Stat.find(params[:id])
    erb :edit
  end
```
  


The second controller action handles the edit form submission. This action responds to a PATCH request to the route /stats/:id. First, we pull the article by the ID from the URL, then we update the weight. The action ends with a redirect to the article show page.
  # update   
```
  patch '/stats/:id' do
    @stat = Stat.find(params[:id])
    @stat.weight = params[:weight]
    @stat.user_id = params[:user_id]
    @stat.save
    erb :show
  end
```

Using PATCH, PUT and DELETE requests with Rack::MethodOverride Middleware

> <form action="/stats/<%= @stat.id %>" method="post">
>   <input id="hidden" type="hidden" name="_method" value="patch">
>  <label>Please Enter Your Weight:</label>
>   <input type="text" name="weight">
>   <input type="submit" value="submit">
> </form>

 

On the stats show page, we have a form to delete it. The form is submitted via a DELETE request to the route /stats/:id. This action finds the stat in the database based on the ID in the url parameters, and deletes it. It then redirects to the index page /stat
  # destroy
```
  delete '/stats/:id/delete' do
    @Stat = Stat.find(params[:id])
    @Stat.destroy
    erb  :delete
  end
end
```






