---
layout: post
title:      "Ruby on Rails Project - Young Papas Hobbies"
date:       2020-04-17 13:18:07 -0400
permalink:  ruby_on_rails_project_-_young_papas_hobbies
---


This project is an oportunity to work with the amazing framework which is Ruby on Rails but also, to build a tool for young fathers like me. So my project is meant to allow young fathers to bring something good out of a challenging experience especially at a young age. They can document their hobbies and build thoughtfull projects. My past projects were related to [business management](https://santiagosalazarpavajeau.github.io/sinatra_cms_portfolio_project_task-process_log) and [finance](https://santiagosalazarpavajeau.github.io/financial_analysis_cli) so choosing this topic was refreshing.

I had my daughter when I was 19 years old and so its been 10 years. So sometimes I wish I could add being a father to my CV, but in the end, all this struggle makes me feel very lucky to have finally found a solid path to my dream career in technology.

We have always heard the saying "there is no manual for being a parent", to this I say there is no manual for most things. But if we create the manual ourselves and build knowledge, hopefully the saying will change to "we build the manual for being a parent" and so I hope this web app reflects that intention developing the knowledge and those skills of learning to solve problems and make clear things that were once vague: both problems and solutions, and at the same time have fun at it.

Now with the technical side the requirements included having Active Record associations between the models. In this case the models are User, Project and Hobby:

* Users have many Projects through Hobbies
* Hobbies have many Projects through Users

For the application to be more complete, projects would have many *Project Updates* which is another model. 

Also developing create, update and destroy functionality within the Model View Conroller system. In this case the model with most functionality is Projects as it has all of the create, update and destroy options built.

The app also has user sign-up and log-in capabilities and also login in from an outside provider which in my case I used Facebook. I used omniauth with tha Facebook API but omniauth supports Google, Github, etc. The user can create a profile, and also delete it through a destroy action. 

On the other hand, the user can log in which is in fact a create action on the Session Controller, and log out which is a destroy action. The User and Session create and destroy actions are tightly related as they depend on each other. For example, when deleting a user from the database with the delete button it is necesary to also delete the user_id on the session hash, otherwise the session will remain for a non-existent user.

The models have regular and custom validations on the models. The validations include prescence, uniqueness, and length for some of the attributes. This gives access to errors and allows the user to correct their when these errors are displayed on their form.

I also added a class model method using some ActiveRecord AREL library methods to access the database quickly. This method allows for the most active user or user with the most posts to be returned. 

```
class Project

       def self.count_by_user
		          all.group(:user_id).count
       end

end

class User

        def self.most_active
                where("id = #{Project.count_by_user.sort_by{|k,v| [-v, k]}.first.first}")
        end

end

```

To break this down a little bit, on the *User.most_active* method I reference the *Project.count_by_user*  method. So the *.count_by_user* method takes all the projects and returns each user_id and how many times it was found in the database. So it returns something of an array of arrays with two numbers the user id and the count.

```
       def self.count_by_user
		           all.group(:user_id).count
       end
			 
			 returns (e.g)
			 
   [ [ 1 ,  3 ] ,   [ 2 ,  5 ] ]  # where the key is the user_id and the value is the count
```

The *.most_active* model class method then finds the user with most projects and returns its object. So we take the count by user array and sort it in descending order of the values so the user with most projects is at the top of the list. Then we have to access the first key value pair and also access the user_id with most projects which is the first value in the array.  With this user_id we can use the *.where* Arel to retrieve the most_active user.

```

        def self.most_active
                  where("id = #{Project.count_by_user.sort_by{|k,v| [-v, k]}.first.first}")
        end

```



I think using Rails helper methods had a learning curve (for example path helpers) but being able to use things like form_for and resources allows to build a powerful app, and allows to see the big picture of building a web app. The main challenges of Rails magic, for me, are dealing with the params hashes so the data goes where it is supposed to and so building nested forms also has a learning curve but it works very well too. 

Lastly an interesting factor that surprised me by the end of the project build was working with front end. I used materialize which I believe is a light version of bootstrap and getting familiar with CSS and HTML was very fast.

[Github Repository](https://github.com/SantiagoSalazarPavajeau/young_papas_hobbies)

[Video Walkthrough](https://youtu.be/1BLh3F6CTUY)

