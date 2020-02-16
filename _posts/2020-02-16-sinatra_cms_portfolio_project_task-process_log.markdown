---
layout: post
title:      "Sinatra CMS Portfolio Project: Task-Process Log"
date:       2020-02-16 17:48:47 -0500
permalink:  sinatra_cms_portfolio_project_task-process_log
---

This project has been one of the most challenging and at the same time rewarding, because it is the first time we get to see and experience the whole idea of developing a web application. I decided to pick an app which required more models than the MVP and by the end realized that there was a reason why the MVP only had limited requirements. However, I think I was able to deliver the functionality I was interested in the begining and found myself trying to make the application more simple in the last days working in the project. I still feel like if I could add some more Front End  customization, *(and also server side)* it would satisfy all of my expectations from the application I wanted to build. In the next section I review the requirements and how I have worked to meet them to submit the portfolio project.

**Requirement Overview
**

In this project we use Sinatra to create a web app that can persist information into a database through ActiveRecord. In my app I used four models *User*, *GlobalProcess*, *Task* and *Job*. As far as relationships, the *User* *has many* of all of the other models, and the rest of the models *belong to* the *User*. Between the other models the relationships are:

* Process has many Jobs through Tasks

* Job has many Processes through Tasks

*  Task belong to Process

*  Task belong to Job

The user signs up with a unique username and password, which is implemented using validations on the users parameters name, email and password. While the user logs in with their unique credentials which is implemented using authentication of their information.  

My belongs to resource that has full CRUD functionality is *GlobalProcess*,  and it is implemented with the corresponding [routes](https://github.com/SantiagoSalazarPavajeau/TASK-PROCESS-LOG/blob/master/app/controllers/global_processes_controller.rb) and [views](https://github.com/SantiagoSalazarPavajeau/TASK-PROCESS-LOG/tree/master/app/views/global_processes). 

Last, the user can only modify their own content through the use of the helper methods *logged_in?* and *current_user*, which only allow entry to these views when the user is in the session or in other words logged in.

[Project Repo](https://github.com/SantiagoSalazarPavajeau/TASK-PROCESS-LOG/tree/master/app/views/global_processes)


