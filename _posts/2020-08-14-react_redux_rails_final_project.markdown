---
layout: post
title:      "React / Redux / Rails Final Project"
date:       2020-08-14 21:31:25 +0000
permalink:  react_redux_rails_final_project
---


On this project I decided to revisit the concept of the Sinatra Task-Process Log but instead of building a Process model, use a Project model. And use the React front end and Rails backend to acheive a nimble prototype web app. Some of the highlights of this prototype include rebuilding a has many to has many relationship "schema" in Redux, using React libraries to make the front end look crisp, and being able to have a running and deployed prototype Rails API with a few CLI commands.

## React - Redux - Thunk


I started building the app on the front end because I knew that this would be the most challenging part especially implementing Redux. But initially, making the style I was going for work was also challening. 

So I ended up building a sidebar and a navbar that aligned together to set the navigation. I think the cool thing about a sidebar is that new views/routes and more functionality can be added and easily accesed. In the future I would like to implement more react libraries that deal with data visualization accessed through links to the sidebar.

I built three reducers on Redux for Projects, Tasks and People. Projects and Tasks have basically full CRUD functionality while People is read only. Implementing CRUD for People could be easily acheived from the Redux and Rails side, but on the React side it would take more work to make it look good. 

I connected the dispatch CRUD actions to the backend using the Thunk middleware.



## Container vs Presentational Components

This was one of the most challenging parts of the build because I used container components for the main models that had access to the Redux store, but the children presentational components seemed to need state due to being forms or needing to create interactivity. The way that I tackled building some stateless presentational components was to build on a very simple element like an input field or a close button. Building purely stateless components is still an area where I have to weigh what is the best possible procedure.

## React Router

The react router was very different to use from the rails router as rails magic builds all the CRUD routes and the controller manages them as well. The method that worked for me to implement the router was to set it on the App.js file where depending on the routes a specific container component would render. To access these routes a react-router Link is needed from anywhere in the app. In this app I built a modal for the show views. This modal is shown on the '/:id' route corresponding to each project. Using a modal might have complicated this area of the app, and some refactoring is possible. 

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

## Conclusion

Working in React allows for great front-end experiences, components from UI libraries can make focusing on functionality possible while making the app look good. Also, using Redux seemed challenging at first, but building complex model relationships can be acomplished following the principles of building the reducers.
