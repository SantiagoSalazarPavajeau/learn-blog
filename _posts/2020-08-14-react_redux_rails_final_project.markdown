---
layout: post
title:      "React Project (+ Redux + Rails)"
date:       2020-08-14 17:31:26 -0400
permalink:  react_redux_rails_final_project
---


On this project I decided to revisit the concept of the Task-Process Log project (written in Ruby) where a user could describe their small business by creating the business processes and the jobs at the company. Then the user was able to relate them by assigning shared tasks by a process and a job. 

But in this React reimagining instead of using a Process model, I used a Project model. I wanted to make an app that was practical for everyday use. React also allowed for a much more user friendly app. 

These tools are interesting to me because I am firm believer that having defined and organised sets of tasks is very valuable when performing complex activities. These tools can allow increasing clarity to work in teams and this keeps everyone happy.

Now, on the technical side, I worked in React for the front end and built a Rails API. Some of the highlights of this prototype include a has many to has many relationship "schema" in Redux, using React libraries to make the front end look crisp, and being able to have a running and deployed prototype Rails API with a few CLI commands.

## React for Style


I started building the app on the front end because I wanted to set up the looks of the app from the start. So I built a sidebar and a navbar that aligned together to handle navigation. I wanted a sidebar because it allows for more views/routes and more functionality to be added and easily accesed. Looking into future challenges I would like to add calendar functionality or data visualization libraries to create a more visual dashboard. 

## Redux - Thunk - Rails for Data

Following the basic set up of the front end style, I built three reducers on Redux for Projects, Tasks and People. The Projects and Tasks models have basically full CRUD functionality while People model is read only. 

These three models are connected though a has many through relationship in Redux and Rails. 

* Projects and People have many Tasks
* Projects have many People through Tasks
* People have many Projects through Tasks
* Tasks belong to Projects and People

The way this is implemented in Redux is by giving the Tasks a *projectid* and *personid* attribute that corresponds to the Project/Person they belong to. In Rails, this is acheived through ActiveRecord relationships.

Finally, I connected the dispatch CRUD actions to the backend using the Thunk middleware.

## Container vs Presentational Components

This was one of the most challenging parts of the build because I used container components for the main models that had access to the Redux store, but the children presentational components seemed to need state due to being forms or needing to create interactivity. The way that I tackled building some stateless presentational components was to build on a very simple element like an input field or a close button.

## React Router

The react router was very different to use from the rails router as rails magic builds all the CRUD/RESTful routes and then we only build the controller. The method that worked for me to implement the react router was to set the Router component on the App.js file and depending on each of the children Route-component's path/route, one of the container components would render. To access these routes a react-router Link is needed from anywhere in the app as a button. In this app I built a modal for the show pages and this modal is shown on the '/:id' route corresponding to each project. 

## Rails

I built the backend for this app last after the front-end was practically done, and Rails magic made it so agile that the following CLI commands got the prototype API running.

```

rails new rails-backend-projects --api --database=postgresql
rails g resource Project title started description:text
rails g resource Person key value text image
rails g resource Task description:text person:belongs_to project:belongs_to completed:boolean
rails db:create 
rails db:migrate 
rails db:seed

rails g serializer Project title started description id created_at updated_at
rails g serializer Person key value text image id created_at updated_at
rails g serializer Task description id person_id project_id completed

```

The command to create a rails project included an ```--api ``` and a ```--database=postgresql``` command that personalized the build for this application to not include rails views(.erb) and to use postgres (heroku deployment). While the resource generators build a model, a controller, a database migration with the specified attributes, and router routes for each of the resources. The controller actions have to be customized manually with this method. Last, the serializer generator creates a file that contains the attributes specified to be sent on the JSON data sent to the front end as a response in redux thunk.

Even deployment was very agile with Rails-Heroku after initializing the project with a Postgres database.

```

heroku create
git push heroku master
heroku run rake db:migrate
heroku run rake db:seed
heroku ps:scale web=1
heroku ps
heroku open

```

## Finale

Working in React allows for great front-end experiences, as using pre-made components from UI libraries allows the app have great looks very swiftly. Leveraging these benefits can give more time to focus on functionality when building an app. Using Redux can allow for building complex model relationships that are managed in the front-end and could be connected to any APIs that fit the needs of the project. 
