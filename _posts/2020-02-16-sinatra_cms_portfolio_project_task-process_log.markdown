---
layout: post
title:      "Sinatra CMS Portfolio Project: Task-Process Log"
date:       2020-02-16 17:48:47 -0500
permalink:  sinatra_cms_portfolio_project_task-process_log
---

This project has been a big challenge, because it was the first time we got to experience the whole concept of developing a web application.

So having a limited sense of what could be done with the amount of time available I picked an ambitious project that got complex really quickly. Basically I wanted to make a platform where collaboration and communication inside a company could take place. In this case that meant for managers and workers to describe their work processes and that way try to align reality with expectations. Also, empower workers to contribute to improve the company saying what could be done better in their everyday tasks. I became interested in these type of platforms while studying business in school, but working at different places also made me realize how important it is for work activities to be explicit for both workers and managers that way training can be better and also everything can be improved. 

Howevera full blown application with this functionality would mean having different types of users and the models would have to have more attributes and this was out of the scope of the current project. The scope of this project waslimited to launching a simple application to get familiar with how a web application works, but I think in the future I would like to develop more features into this app and fulfill my initial idea.

Even presently (limiting features to several models and only one user) my app ended up having more models than the requirement and by the end realized that there was a reason why we were encouraged to keep it simple. However, I think I was able to deliver some of the functionality I was interested in and even then found myself trying to make the application more simple in the last days working in the project. In the next section I review the requirements and how I have worked to meet them to submit the portfolio project.

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


