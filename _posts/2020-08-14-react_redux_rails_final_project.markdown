---
layout: post
title:      "React Project (+ Redux + Rails)"
date:       2020-08-14 17:31:26 -0400
permalink:  react_redux_rails_final_project
---


On this project I decided to revisit the concept of the [Task-Process Log project](https://santiagosalazarpavajeau.github.io/sinatra_cms_portfolio_project_task-process_log) (written in Ruby) where users can describe their small business by defining business processes and jobs at the company, and relating these by shared tasks (to set specifically who does what in the business). But in this React rewrite instead of using a Process model, I use a Project model which makes it more practical for everyday use.

I have been very interested in these kinds of tools because I am firm believer that having defined, clear, and organised sets of tasks keeps everyone happy in collaborative projects and work. Plus React makes the front-end very user-friendly.

Now, focusing on the technical side, I worked in React for the front end and paired it with a Rails API. Some of the highlights of this prototype include:

* Building model relationships in Redux and Rails.
* Creating async actions through Redux Thunk.
* Applying container and presentational component pattern.
* Implementing Redux and Hooks.
* Authentication using JWT, and other Ruby gems.
* Deploying a Rails API to Heroku, and the React front end to Github Pages.

## Hooks - Redux - Thunk & Rails for Data Storage

Following the basic set up of the front end UI, I built three reducers on Redux for Projects, Tasks and People, that communicate with the backend to store, edit or delete data. This setup allows to have access to temporary data in the form of "state", like the a command to open/close a pop up, but also allows to store the project data permanently in the Postgres database. 

### Synchronizing Redux and Rails

Specifically, the three models are connected though a has many through relationship in Redux and Rails. 

* Projects and People have many Tasks
* Projects have many People through Tasks
* People have many Projects through Tasks
* Tasks belong to Projects and People

The way this is implemented in Redux is by giving the Tasks a *project_id* and *person_id* attribute that corresponds to the Project/Person they belong to. In Rails, this is acheived through ActiveRecord relationships.

Finally, I connected the dispatch CRUD actions to the backend using the Thunk middleware.

### Using Hooks

Initially, I wrote the app with class components and then refactored to use Hooks. I used `useState` to replace the state class syntax and `useEffect` to replace class lifecycle methods. Also, I was able to refactor the verbose `connect` redux function by using `useSelector` to access the store and `useDispatch` to access the actions.

## Container vs Presentational Components

This was one of the most challenging parts of the build because I used container components for the main models that had access to the Redux store, but the children presentational components seemed to need complex state due to being having complex forms for easier user interactivity. The way that I initially tackled building some stateless presentational components was to build on a very simple element like an input field or a close button. But with hooks it was much more easy to simplify components.

## React Router

The react router was very different to use from the rails router as rails magic builds all the CRUD/RESTful routes and then we only build the controller. The method that worked for me to implement the react router was to set the Router component on the App.js file and depending on each of the children Route-component's path/route, one of the container components would render. To access these routes a react-router Link is needed from anywhere in the app as a button. Setting up the view page of each project as a modal without having to change or build a complex route allowed for the deployment version to be compatible with the functionality I had in development.

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

Working in React allows for great front-end experiences, and using the available CSS UI components from external libraries allows the app have great looks very swiftly and can give more time to focus on functionality when building an app. 

Using Redux can allow for building complex model relationships that are managed in the front-end and could be connected to any APIs that fit the needs of the project. Hooks really simplify components and get rid of a lot of verbose that come with class-based React.

[Live Project Manager App](https://santiagosalazarpavajeau.github.io/react-projects/#/projects) : [link](https://santiagosalazarpavajeau.github.io/react-projects/#/projects)

[GitHub Repo](https://github.com/SantiagoSalazarPavajeau/react-projects): [link](https://github.com/SantiagoSalazarPavajeau/react-projects)
