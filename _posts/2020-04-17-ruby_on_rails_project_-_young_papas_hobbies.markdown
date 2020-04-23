---
layout: post
title:      "Ruby on Rails Project - Young Papas Hobbies"
date:       2020-04-17 13:18:07 -0400
permalink:  ruby_on_rails_project_-_young_papas_hobbies
---


This project is an oportunity to work with the amazing framework which is Ruby on Rails but also, to build a tool for young fathers like me. So my project is meant to allow young fathers to bring something good out of a challenging experience especially at a young age. They can document their hobbies and build thoughtfull projects. My past projects were related to documenting [Tasks-Processes](https://santiagosalazarpavajeau.github.io/sinatra_cms_portfolio_project_task-process_log) and a [financial analysis CLI](https://santiagosalazarpavajeau.github.io/financial_analysis_cli) so choosing this topic was diferent and refreshing because is something that relates more to everyday life.

I had my daughter when I was 19 years old and so its been 10 years. So sometimes I wish I could add being a father to my CV, but in the end, all this struggle makes me feel very lucky to have finally found a solid path to my dream career in technology.

We have always heard the saying "there is no manual for being a parent", to this I say there is no manual for most things. But if we create the manual ourselves and build knowledge, hopefully the saying will change to "we build the manual for being a parent" and so I hope this web app reflects that intention developing the knowledge and those skills of learning to solve problems and make clear things that were once vague: both problems and solutions, and at the same time have fun at it.

Now with the technical side the requirements included having Active Record associations between the models. In this case the models are User, Project and Hobby:

* Users have many Projects through Hobbies
* Hobbies have many Projects through Users

For the application to be more complete, projects would have many *Project Updates* which is another model. 

Also developing create, update and destroy functionality within the Model View Conroller system. In this case the model with most functionality is Projects as it has all of the create, update and destroy options built.

The app also has user sign-up and log-in capabilities and also login in from an outside provider which in my case I used Facebook. 

Through Facebook we obtiain a *auth* hash that works like the *params* hash. This *auth* can be used to create our user as it has name, email and image attributes sent over through Facebook. 

```
            @user = User.find_or_create_by(username: auth['info']['name'])
            @user.password = SecureRandom.hex(9)
						
            if @user.save
                session[:user_id] = @user.id
                redirect_to @user
            else
                redirect_to root_path
            end
```

There can be different ways to create the user however in my case I used the name attribute and also set a 9 digit random password so the new user passes the validations of has_secure_ password. Then I set the session to the id created by ActiveRecord. 

I used omniauth with the Facebook API but omniauth supports Google, Github, etc. The user can create a profile, and also delete it through a destroy action. 

On the other hand, the user can log in which is in fact a create action on the Session Controller, and log out which is a destroy action. The User and Session create and destroy actions are tightly related as they depend on each other. For example, when deleting a user from the database with the delete button it is necesary to also delete the user_id on the session hash, otherwise the session will remain for a non-existent user.

```
   def destroy
        session.delete :user_id
        @user.delete 
        redirect_to root_path
    end
```

The models have regular and custom validations on the models. The validations include prescence, uniqueness, and length for some of the attributes. This gives access to errors and allows the user to correct their when these errors are displayed on their form.

I also added a class model method using some ActiveRecord AREL library methods to access the database quickly. This method allows for the top 3 most active users to be returned. 

To get this working I used the *count cache* functionality which requires you to add a new column on the database you want to implement it and so you start by adding a count cache to the model being counted:

```
class Project < ApplicationRecord
    belongs_to :user, counter_cache: true
end
```

Then I ran the migration:

```
rails g migration add_projects_counter_cache_to_users projects_count:integer
```

And reset the counter in rails console in order to evade dropping my database.

```
User.all.each do |user|
  User.reset_counters(user.id, :projects)
end
```

credits to: Dakota Martinez for the tip!

After this my objects contained a new attribute name projects_count, that is updated every time the user creates a new project on the app.

```
#<User id: 1, username: "SantiSalazar", password_digest: [FILTERED], bio: "Father at 19 years old...Turned Software Developer", created_at: "2020-04-11 17:43:46", updated_at: "2020-04-11 17:43:46", email: "santisalazar@yph.com", uid: nil, projects_count: 4>
```


I think using Rails helper methods had a learning curve (for example path helpers) but being able to use things like form_for and resources allows you to build a powerful app, and also to see the big picture of building a web app with less technical load. 

For example, on my [Sinatra project building an embeded form](https://github.com/SantiagoSalazarPavajeau/TASK-PROCESS-LOG/blob/master/app/views/jobs/edit.erb) was not very DRY and the plain html form had to have all of the information regarding the external model in the form. However, on rails the form details are created for us by only referencing the external model. 

```
*/views/projects/_form.html.erb


<%= form_for @project do |f|%>

<%=f.label :hobby_title%>
    <%= f.text_field :hobby_title, list: "hobbies_autocomplete" %>
        <datalist id= "hobbies_autocomplete"> 
            <%Hobby.all.each do |hobby|%>
                <option value="<%= hobby.title%> ">
            <%end%>
        </datalist>
				
...
				
				 <%= f.submit%>

<% end%>

```

In this case I am creating a Hobby object from a autocomplete list and the field gives you the option to choose from an existing Hobby or to create a new one, and the rails magic makes it work. The main challenges of Rails magic, for me, are dealing with the params hashes so the data goes where it is supposed to and building nested forms. On the embeded form_for example I pass a hobby title attribute to the params hash like so:

```
*/controllers/projects_controller.rb

def create
        @project = current_user.projects.create(project_params)
end
...

def project_params
        params.require(:project).permit(:title, :description, :hobby_title, :image)
end

```

And rails generates the following form input tag:

```

<input list="hobbies_autocomplete" type="text" name="project[hobby_title]" id="project_hobby_title">

<datalist id="hobbies_autocomplete"> 
 <option value="..."> </option>
 ...
```

So the hobby_title attribute is allowing for a attribute of another model to be sent through the params hash.  If we look at the params hash for a *project_params*:

```

{"title"=>"steak", "description"=>"best steak ever in the world using wayuu steak", "hobby_title"=>"Cooking"}

```

It has an attribute literally called hobby_title. But if we look at the project created by this *project_params* it is:

```
class Project

    def hobby_title=(title)
        self.hobby = Hobby.find_or_create_by(title: title.strip)
    end

    def hobby_title
        self.hobby ? self.hobby.title : nil
    end

end
```

So in reality we are skipping the *accepts_nested_attributes*, but when we use the attribute :hobby_title on the input tag, the project method recieves it as an instance method so it can create the hobby object.


Lastly an interesting factor that surprised me by the end of the project build was working with front end. I used materialize which I believe is a light version of bootstrap and getting familiar with CSS and HTML was very fast, and I ended up being very satisfied with my front end. Simple things like a Nav bar and buttons make everything much more organized and makes you look forward to React.

[Github Repository](https://github.com/SantiagoSalazarPavajeau/young_papas_hobbies)

[Video Walkthrough](https://youtu.be/1BLh3F6CTUY)

