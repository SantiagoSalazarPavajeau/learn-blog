---
layout: post
title:      "From the user to the database and back: steps "
date:       2020-01-13 15:47:51 -0500
permalink:  from_the_user_to_the_database_and_back_process_aproach
---

When the user inputs data into a website using Ruby/Sinatra the data travels from HTML to Ruby and to SQL. In fact, Ruby uses Sinatra for get and post requests, then ActiveRecord and ORM to instantiate objects and also to connect the data to an SQL database. The following code snippets describe these steps in the process of saving user input into our program and also even showing a dynamic response to the user input. 

The user inputs data into a form from a website:

<form method="POST" action="/site">
      <input type="text" name="attribute">
      <input type="submit">
</form>

![](https://photos.app.goo.gl/DTjLPuUP3PJJk9Qu9)

And the html to acheive this should look like:

```
<form method="POST" action="/site">
      <input type="text" name="attribute">
      <input type="submit">
</form>
```

ERB allows for the transfer of data from the browser or front end HTML code to ruby.

```
params[:attribute] = "user input"

```

Sinatra connects the client and the server:

```
get "/"
   erb: html_and_ruby_file_name
end
```

ActiveRecord saves the data to the database.

```
Model.create(params[:attribute])
```

Ruby instantiates an object.

```
class Model

     attr_accessor :attribute

     def self.method
            self.first
     end

end
```

ORM of the method uses SQL to map the object to the database.

```
sql = "CREATE TABLE models (  id  INTEGER PRIMARY KEY, 
attribute TEXT)"

DB[:conn].execute(sql)

sql = "INSERT INTO models (attribute) VALUES ('user input')"

DB[:conn].execute(sql)

```

SQL saves the user input into the SQL database.

To ouput information from out app we can use a Sinatra POST request

```
post "/"
     Model.method
		 erb: htm_and_ruby_file_name
end
```

And send the ruby instance variable into the front end on the HTML through the ERB file:

```
<!DOCTYPE html>
<html>
  <head>
   
  </head>

  <body>
    <h1>Title</h1>

    <h2><%= Model.method %></h2>
  </body>
</html>
```



 
