---
layout: post
title:      "Ruby CLI Gem Project "
date:       2019-11-25 20:44:01 -0500
permalink:  financial_analysis_cli
---

In this project we were required to create a ruby gem to showcase the use of object orientation, scraping/APIs to obtain data and also the development of a command line interface for user interaction. So in the end this project can be robust in the handling of data but in terms of user interaction is very primitive. 

First focusing on what type of data I was going to obtain, at first I considered scraping a social media site to find the newest clothing or shoes, however, scraping this information was not the best option as the site has an API, but in the end I was not able to find access to the site's oficial API. Then, I started to do some more research and got tips about diferent APIs like Dubai open API and BBVA open API as well, and the latter definitely started to interest me more. However their API is mostly focused on providing payment processing applications to white label banks, and also payment processing for companies. Then I found one startup that provides an accounting API to businesses and that made me think about financial data APIs, so I was able to find the one used in the project that is offered by a website that provides information about financial markets. I ended up choosing this API because it was free and open, and the calls had easily accesible data that was perfect to create this type of CLI gem and fullfil the requirements. 

So this gem allows the user to access information about public companies from several command line prompts. At first, it displays a list of the most traded companies of the day and allows the user to obtain more information about the company of their choice. I connected several API calls in order to make information about the company easily accesible with few commands. So even though the interaction with the user is fairly primitive I believe it is not complicated and allows an in depth overview of the company with only 2 levels of data.

The information is obtained using a financial markets API call that contains a list of the 10 most active companies of the day. We then extract the stock-tickers and create 10 instances of the Company object with another API call.

The tickers extracted from the first API response are inserted into another API call in order to obtain more information about each individual company and each attribute is set to the information available in this 'profile' response. 

Some of the values available in the dataset are company name, stock price, company description, CEO, etc. However, more in depth financial data could be displayed such as the balance sheets and the income statements for the last 10 years.

Object Orientation is applied in this gem through 3 classes: Company, API and CLI.  

The Company class initializes Company objects that carry the final information to the user. This information is obtained by obtaining an API response upon initialization and using the .send method to create an Company object attribute for each data point in the API response. If multiple instances of the Company class are created, they are stored in the .all class variable. At first I only used the information from the profile API response, however I was able to expand the amount of information to be displayed by adding the balance sheet and income statement data to the company object when initialized. This slowed down the program by a little bit but I think the gem feels more complete and more unique when all of this information is available. Also, the information available could be expanded to include things like cash flow statements, or also to be able to call an object on other companies that are not on the list. But the amount of information that I accessed I think proves the point that more information can be accessed.

The API class makes the first call get the top 10 most active companies, and returns an array of hashes with the data. The tickers contained in this array of hashes are used to create the 10 objects which are then available to access the information about these 10 companies. 

The CLI class then serves to allow a user to interact with the data using simple text commands, namely the ticker, list or exit. Also after the user has chosen a company to get more information on, the user can choose to obtain the balance sheet and income statement data.

So even though this CLI Gem Project is very simple I think it could be an interesting tool for students that need to access financial information about companies, but also for other types of analysis since it could be a good starting point to access a bigger amount of data about companies. 

Overall, this project was a very rewarding experience and eventhough it was very simple in the end, I feel like I was lucky to find a good API that allowed me to have access to the information in such a straight forward way. However, it did take some tinkering and refactoring of the classes and methods to make it as efficient as it is. At first, I was considering creating a balance sheet class and income statement class, but with the guidance of my coach we realized that maybe this was not helping the app fulfill the requirements which were more focused on accessing one level deep of data using object oriented ruby and not exactly in complex object collaboration and relationships.

I am very pleased with the outcome of the project and feel like I have learned so much about object oriented ruby,  procedural ruby, and CLI applications only after a little bit less than two month of learning ruby. 




