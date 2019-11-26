---
layout: post
title:      "Financial Analysis CLI"
date:       2019-11-26 01:44:00 +0000
permalink:  financial_analysis_cli
---


(IN CLI file and Terminal)
This gem allows the user to access information about public companies. 

It displays a list of the most traded companies of the day and allows the user to obtain more information about the company of their choice. We connect several API calls in order to make information about the company easily accesible with a few commands.

(IN API file)
The information is obtained using a financial markets API call that contains a list of the 10 most active companies. We then extract the stock-tickers and create 10 instances of the Company object with another API call.

(IN Company file)
The tickers extracted from the first API response are inserted into another API call in order to obtain more information about each individual company and each attribute is set to the information available in this 'profile' response. 

Some of the values available in the dataset are company name, stock price, company description, CEO, etc. However, more in depth financial data could be displayed such as the balance sheets and the income statements for the last 10 years.

Object Orientation is applied in this gem through 3 classes: Company, API and CLI.  

The Company class initializes Company objects that carry the final information to the user. This information is obtained by obtaining an API response upon initialization and using the .send method to create an Company object attribute for each data point in the API response. If multiple instances of the Company class are created, they are stored in the .all class variable.

The API class makes the first call get the top 10 most active companies, and returns an array of hashes with the data. The tickers contained in this array of hashes are used to create the 10 objects which are then available to access the information about these 10 companies. 

The CLI class then serves to allow a user to interact with the data using simple text commands, namely the ticker, list or exit.


