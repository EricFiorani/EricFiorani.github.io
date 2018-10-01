---
layout: post
title:      "Sinatra Project - The WOD App"
date:       2018-10-01 15:44:39 +0000
permalink:  sinatra_project_-_the_wod_app
---


Although back-end programming does not follow my interests, the sinatra-activerecord project was more entertaining and eye opening then the scraper project!

To start, before I enter into the mechanics of this project app, [The WOD App](https://github.com/EricFiorani/sinatra-project-wod-app); I've realized the importance of learning back-end code even when wanting to be a front-end developer because theres no denying that the two sides of programming eventually have to meet somewhere in the middle to create something thats fully functional.

As the learn cirriculum starts adding in front-end UI to its project criteria, the more entertaining I've found it. Lets get into the technical side now.

## The app is written in Sinatra and utilizes the MVC pattern. 

* **Models**: The 'logic' of a web application. This is where data is manipulated and/or saved.

* **Views**: The 'front-end', user-facing part of a web application - this is the only part of the app that the user interacts with directly. Views generally consist of HTML, CSS, and Javascript.

* **Controllers**: The go-between for models and views. The controller relays data from the browser to the application, and from the application to the browser.


The Models of the WOD app are broken down into **Users** and **Wods** (Workouts of the day). Users have many WODs (has_many relationship) and WODs belongs to a User (belongs_to relationship).


The Views are broken up into a main index, WODs directory for creating, editing, showing, different WOD logs and a Users directory for registration, logging in and showing username profile which will be accessed through validation login (more on that later).


The Controllers are broken down into 3 seperate controllers. Application controller (main), Users controller and a WOD controllers. This helps organize and seperate various routes and methods to prevent one large controller file that would be harder to sift through.

![](https://i.imgur.com/oSoSGvjl.png)



## Using ActiveRecord with Sinatra

The purpose of ActiveRecord is to use the CRUD method (Create, Read, Update, Delete). This will create the database that information will be stored in. ActiveRecord works in conjunction with Sinatra. Sinatra allows a user to implement these actions through the interface of the web. In combination creates the app that I'm delivering today. 

Essentially Sinatra builds the routes for the methods to be executed from ActiveRecord onto an interface for the user to interact with.

### As part of the FlatIron Cirriculum, it works as follows - 

* **Create**
The "create" part of CRUD is implemented in Sinatra by building a route, or controller action, to render a form for creating a new instance of your model.

* **Read**
There are two ways in which we can read data. We may want to "read" or deliver to our user, all of the instances of a class, or a specific instance of a class. (ie. get '/models' or get '/models/:id)

* **Update**
To implement the update action, we need a controller action that renders an update form, and we need a controller action to catch the post request sent by that form.

* **Delete**
It doesn't get its own view page but instead is implemented via a "delete button" on the show page of a given instance. This "delete button", however, isn't really a button, it's a form. The form should send a DELETE request to delete '/models/:id/delete' and should contain only a "submit" button with a value of "delete".


## Has user accounts with the ability to register and log in.

Upon opening the index page or 'homepage' of the app, you will be directed to login or signup to continue into the app. 

**Signing Up** - Users have to have a unique username to pass validation.

**Logging In** - Once signed up, there are some validation features to prevent users from getting into another's profile.

When putting in the wrong user information, you will be encountered with an error message since the information could authenticate. This is done by - 

`post '/login' do
    
	@user = User.find_by(username: params[:username])
    if @user && @user.authenticate(params[:password])
      session[:user_id] = @user.id
      redirect to "/users/#{@user.slug}"
			
    else
      flash[:message] = "***Incorrect username or password, please try again!***"
      erb :"/users/login"
			
    end
  end`

## User can't modify content created by other users.

Another necessary proponent of validation is modifying WOD logs. Obviously, a random user should not be able to edit your WOD.  This is achieved by matched the current users user_id, with the user_id matched to that specific WOD log.

This is done via - 

```get '/wods/:id/edit' do

    @wod = Wod.find_by(id: params[:id]) 
    if current_user.id == @wod.user_id
      erb :'/wods/edit_wod'
    else
      flash[:edit] = "***That Is Not Yours. You Can Not Edit A Post That Doesn't Belong To You!***"
      erb :'/wods/show_wod'
			
    end
  end
	```
	
	






