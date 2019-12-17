---
layout: post
title:      "Public Company Data Ruby CLI Gem Project "
date:       2019-11-25 20:44:01 -0500
permalink:  financial_analysis_cli
---

In this project, we were required to create a ruby gem to show the use of object-orientation, scraping/APIs and the command line. So the gem had to be able to provide information to the user through the command line and this information had to be extracted from the web, then stored in instances of objects. In the end, I think these requirements allowed it to be innovative in terms of fetching data but as far as user interaction it was quite simple. 

```
Time for some Financial Analysis
1. National Western Life Group Inc. -> ticker: NWLI
2. Humana Inc. -> ticker: HUM
3. Reata Pharmaceuticals Inc. -> ticker: RETA
4. Chipotle Mexican Grill Inc. -> ticker: CMG
5. Paycom Software Inc. -> ticker: PAYC
6. Amazon.com Inc. -> ticker: AMZN
7. Booking Holdings Inc. -> ticker: BKNG
8. Charter Communications Inc. -> ticker: CHTR
9. Graham Holdings Company -> ticker: GHC
10. Rogers Corporation -> ticker: ROG
Choose a company you would like more information on by typing its ticker (e.g. AAPL). Type list to see some available companies or type exit.
```

At first, focusing on what type of data I was going to obtain, I considered scraping a social media site to find the latest clothing or shoes, however, scraping this information was not the best option as the site that I was first looking at (Pinterest) has a private API, so in the end I was not able to acquire access to the site's official API. Then, I started to do some more research and got tips about different free and open APIs like Dubai API and BBVA API and the BBVA API definitely started to interest me more as it made me think that obtaining financial data could be a solid approach to the project given that it is so clear and straightforward to obtain. Then I found one startup that provides an accounting API to businesses and that made me think about obtaining data about balance sheets, income statements, etc. and, so I was lucky to find a great API offered by a website that provides information about financial markets and public companies. This API was free and open, and the calls had easily accessible data that was a great fit to create the CLI gem and fulfill the requirements. 

```
response = HTTParty.get("https://financialmodelingprep.com/api/v3/company/profile/#{ticker}")
```

So the final gem I created allows the user to access information about public companies from several command-line prompts. At first, it displays a list of the most traded companies of the day and allows the user to obtain more information about the company of their choice. I connected several API calls in order to make information about the company easily accessible with few commands. So even though the interaction with the user is fairly simple I believe it is not complicated and allows an in-depth overview of the company with only 2 levels of data.

```
Choose a company you would like more information on by typing its ticker (e.g. AAPL). Type list to see some available companies or type exit.
CMG

Chipotle Mexican Grill Inc.:

Chipotle Mexican Grill Inc together with its subsidiaries operates Chipotle Mexican Grill restaurants, which serves a menu of burritos, tacos, burrito bowls and salads, made using fresh ingredients.

Sector: Consumer Cyclical
Industry: Restaurants
Website: http://www.chipotle.com
Stock price: 832.21
Market capitalization: 23154578830.00

Would you like to see the balance sheet and income statment of this company? y/n
y
Balance Sheet:
{"date"=>"2018-12-31",
 "Cash and cash equivalents"=>"280152000.0",
 "Short-term investments"=>"426845000.0",
 "Cash and short-term investments"=>"706997000.0",
 "Receivables"=>"62312000.0",
 "Inventories"=>"21555000.0",
 "Total current assets"=>"814794000.0",
 "Property, Plant & Equipment Net"=>"1379254000.0",
 "Goodwill and Intangible Assets"=>"21939000.0",
 "Long-term investments"=>"0.0",
 "Tax assets"=>"0.0",
 "Total non-current assets"=>"1450724000.0",
 "Total assets"=>"2265518000.0",
 "Payables"=>"113071000.0",
 "Short-term debt"=>"0.0",
 "Total current liabilities"=>"449990000.0",
 "Long-term debt"=>"0.0",
 "Total debt"=>"0.0",
 "Deferred revenue"=>"70474000.0",
 "Tax Liabilities"=>"16695000.0",
 "Deposit Liabilities"=>"0.0",
 "Total non-current liabilities"=>"374189000.0",
 "Total liabilities"=>"824179000.0",
 "Other comprehensive income"=>"-6236000.0",
 "Retained earnings (deficit)"=>"2573617000.0",
 "Total shareholders equity"=>"1441339000.0",
 "Investments"=>"426845000.0",
 "Net Debt"=>"-706997000.0",
 "Other Assets"=>"23930000.0",
 "Other Liabilities"=>"336919000.0"}
Income Statment:
{"date"=>"2018-12-31",
 "Revenue"=>"4864985000.0",
 "Revenue Growth"=>"0.0868",
 "Cost of Revenue"=>"3953993000.0",
 "Gross Profit"=>"910992000.0",
 "R&D Expenses"=>"0.0",
 "SG&A Expense"=>"375460000.0",
 "Operating Expenses"=>"652624000.0",
 "Operating Income"=>"258368000.0",
 "Interest Expense"=>"0.0",
 "Earnings before Tax"=>"268436000.0",
 "Income Tax Expense"=>"91883000.0",
 "Net Income - Non-Controlling int"=>"0.0",
 "Net Income - Discontinued ops"=>"0.0",
 "Net Income"=>"176553000.0",
 "Preferred Dividends"=>"0.0",
 "Net Income Com"=>"176553000.0",
 "EPS"=>"6.35",
 "EPS Diluted"=>"6.31",
 "Weighted Average Shs Out"=>"27787300.0",
 "Weighted Average Shs Out (Dil)"=>"27823000.0",
 "Dividend per Share"=>"0.0",
 "Gross Margin"=>"0.1873",
 "EBITDA Margin"=>"0.097",
 "EBIT Margin"=>"0.0552",
 "Profit Margin"=>"0.036",
 "Free Cash Flow margin"=>"0.0687",
 "EBITDA"=>"470415000.0",
 "EBIT"=>"268436000.0",
 "Consolidated Income"=>"176553000.0",
 "Earnings Before Tax Margin"=>"0.0552",
 "Net Profit Margin"=>"0.0363"}
```

The information is obtained using a financial markets API call that contains a list of the 10 most active companies of the day. We then extract the stock-tickers and create 10 instances of the Company object with another API call.

The tickers extracted from the first API response are inserted into another API call in order to obtain more information about each individual company and each attribute is set to the information available in this 'profile' response. 

```
https://financialmodelingprep.com/api/v3/company/profile/AAPL
```

Outputs:

```
{
symbol: "AAPL",
profile: {
price: 281.2,
beta: "1.139593",
volAvg: "36724977",
mktCap: "1298511831630.00",
lastDiv: "2.92",
range: "142-233.47",
changes: -0.03,
changesPercentage: "(-0.01%)",
companyName: "Apple Inc.",
exchange: "Nasdaq Global Select",
industry: "Computer Hardware",
website: "http://www.apple.com",
description: "Apple Inc is designs, manufactures and markets mobile communication and media devices and personal computers, and sells a variety of related software, services, accessories, networking solutions and third-party digital content and applications.",
ceo: "Timothy D. Cook",
sector: "Technology",
image: "https://financialmodelingprep.com/images-New-jpg/AAPL.jpg"
}
}
```

Some of the values available in the dataset are the company name, stock price, company description, CEO, etc. However, more in-depth financial data can be displayed such as the balance sheets and the income statements for the last 10 years using other API calls.

Object Orientation is applied in this gem through 3 classes: Company, API and CLI.  

The Company class initializes Company objects that carry the final information to the user. This information is obtained through an API response upon initialization and set through the .send method to create a Company object attribute for each data point in the API response. If multiple instances of the Company class are created, they are stored in the .all class variable. 

```
def initialize(ticker)
    @ticker = ticker
    response = HTTParty.get("https://financialmodelingprep.com/api/v3/company/profile/#{ticker}")
    response["profile"].each { |k,v| self.send("#{k}=", v) }
    response_balance = HTTParty.get("https://financialmodelingprep.com/api/v3/financials/balance-sheet-statement/#{ticker}")
    self.balance_sheets = response_balance["financials"][0]
    response_income = HTTParty.get("https://financialmodelingprep.com/api/v3/financials/income-statement/#{ticker}")
    self.income_statements = response_income["financials"][0]
    @@all << self
  end
```

At first, I only used the information from the profile API response, however, I was able to expand the amount of information to be displayed by adding the balance sheet and income statement data to the company object when initialized. This slowed down the program by a little bit but I think the gem feels more complete and more unique when all of this information is available. Also, the information available could be expanded to include things like cash flow statements, or also to be able to initialize an instance of the Company class from other companies that are not on the list. But the amount of information that I provide, for now, I believe proves the point that a large amount of information can be accessed in this manner.

The API class makes the first call to get the top 10 most active companies, and we use this response to create an array of hashes with the tickers. The tickers contained in this array of hashes are used to create the 10 Company objects which are where the information is stored and available to the user.

```
def self.get_companies
    response = HTTParty.get("https://financialmodelingprep.com/api/v3/stock/actives")
    response["mostActiveStock"].each do |hash|
      FinancialAnalysis::Company.new(hash["ticker"])
    end
  end
```

The CLI class then serves to allow a user to interact with the data using simple text commands, namely the ticker, list or exit. Also after the user has chosen a company to get more information on, they can choose to obtain the balance sheet and income statement data.

```
def start
    puts "Time for some Financial Analysis"
    FinancialAnalysis::API.get_companies
    list_companies
    menu
  end
  
  def list_companies
    @companies = FinancialAnalysis::Company.all
    @companies.each.with_index(1) do |company, i|
      puts "#{i}. #{company.companyName} -> ticker: #{company.ticker}"
    end
  end
  
  def menu
    input = nil
    while input != 'exit'
    puts "Choose a company you would like more information on by typing its ticker (e.g. AAPL). Type list to see some available companies or type exit."
      input = gets.strip
      if input == 'list'
        list_companies
      elsif @companies.detect{ |company| company.ticker == input}
        company = @companies.detect{ |company| company.ticker == input}
         puts ""
         puts "#{company.companyName}:" if company.companyName != ""
         puts ""
         puts "#{company.description}" if company.description != ""
         puts ""
         puts "Sector: #{company.sector}" if company.sector != ""
         puts "CEO: #{company.ceo}" if company.ceo != ""
         puts "Industry: #{company.industry}" if company.industry != ""
         puts "Website: #{company.website}" if company.website != ""
         puts "Stock price: #{company.price}" if company.price != ""
         puts "Market capitalization: #{company.mktCap}" if company.mktCap != ""
         puts ""
         puts "Would you like to see the balance sheet and income statment of this company? y/n"
          input = gets.strip.downcase
          if input == "y"
            puts "Balance Sheet:" 
            Pry::ColorPrinter.pp(company.balance_sheets)
            puts "Income Statment:"
            Pry::ColorPrinter.pp(company.income_statements)
          else
          end
      else
        puts "You can retry typing a company's ticker, or 'list', or 'exit'."
      end
    end
  end
```

So even though this CLI Gem Project is very simple I think it could be an interesting tool maybe for students that need to access financial information about companies for school work, but also even for professional analysts since it could be a good starting point to acquire more thorough data about companies. 

Overall, this project was a very rewarding experience and in the end, the result was very simple. I feel that I was lucky to find a good API that allowed me to have access to the information in such a straight forward way. However, it did take some tinkering and refactoring of the classes and methods to make it as efficient as I think it is. At first, I was considering creating a balance sheet class and income statement class, but with the guidance of my coach realized that maybe this was not helping the app fulfill the requirements which were focused on accessing one level of data using object-oriented ruby and not in complex object collaboration and relationships.

I am very pleased with the outcome of the project and feel like I have learned so much about object-oriented ruby, procedural ruby, and CLI applications only after a little bit less than two months of learning ruby. 

### [Link to GitHub repository](https://github.com/SantiagoSalazarPavajeau/financial_analysis_cli)





