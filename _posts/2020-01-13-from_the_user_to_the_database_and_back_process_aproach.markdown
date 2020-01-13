---
layout: post
title:      "From the user to the database and back: process aproach"
date:       2020-01-13 15:47:51 -0500
permalink:  from_the_user_to_the_database_and_back_process_aproach
---

When the user inputs data into a website using several the data travels from HTML to Ruby and to SQL. Moreover, in ruby we use Sinatra for get and post requests, then ActiveRecord and ORM to instantiate ruby objects and finally connect the data to an SQL database. The following code snippets describe these steps in the process of saving user input into our program and also even showing a dynamic response to the user input. 

The user inputs data into a form from a website and 

![](https://photos.google.com/search/_tra_/photo/AF1QipNzc2hfx_pyDVZ8_frXhTAB5fsGgYVB5qxyWvy6http://)

the html to acheive this looks like:

```
<form method="POST" action="/reverse">
      <input type="text" name="string">
      <input type="submit">
</form>
```

ERB allows for the transfer of data from front end to ruby.

```
<!DOCTYPE html>
<html>
  <head>
  </head>

  <body>
    <h1>Title</h1>

    <h2><%= @ruby_instances_and_methods %></h2>
  </body>
</html>

```

Sinatra connects the client and the server:

```
get "/"
   erb: file_name
end
```


Ruby instantiates an object.

```
class Model

     def method
            @ruby_instances_and_methods
     end

end
```

ActiveRecord sends the data to the database.

```
Model.create(params[:attribute])
```

ORM of the method uses SQL to run the method.

```
sql = "CREATE TABLE models (  id  INTEGER PRIMARY KEY, 
attribute TEXT)"

DB[:conn].execute(sql)

sql = "INSERT INTO models (attribute) VALUES (user_input)"

DB[:conn].execute(sql)

```

SQL saves the user input into the SQL database.

To ouput information from out app we can use a Sinatra POST request

```
post "/"
     @ruby_instances_and_methods
		 erb: file_name
end
```



 
