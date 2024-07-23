### Web_Scraping_Challenge

# Background 
You’re now ready to take on a full web-scraping and data analysis project. You’ve learned to identify HTML elements on a page, identify their id and class attributes, and use this knowledge to extract information via both automated browsing with Splinter and HTML parsing with Beautiful Soup. You’ve also learned to scrape various types of information. These include HTML tables and recurring elements, like multiple news articles on a webpage.

## Important Vocabulary
### Splinter- a Python framework that provides a simple and consistent interface for web application automation. Key features: Easy to learn: The API is designed to be intuitive and quick to pick up. Faster to code: Automate browser interactions quickly and reliably without fighting the tool. 

### Html Parsing-  means analyzing and converting a program into an internal format that a runtime environment can actually run, for example the JavaScript engine inside browsers. The browser parses HTML into a DOM tree. HTML parsing involves tokenization and tree construction. 

### Beautiful Soup- is a library that makes it easy to scrape information from web pages. It sits atop an HTML or XML parser, providing Pythonic idioms for iterating, searching, and modifying the parse tree.

# Instructions 
This new assignment consists of two technical products. You will submit the following deliverables:

# You need to import Splinter and Beautiful Soup
 from splinter import Browser
from bs4 import BeautifulSoup

#Connect to your browser
browser = Browser('chrome')

    Deliverable 1: Scrape titles and preview text from Mars news articles.
    # Visit the website
# https://static.bc-edx.com/data/web/mars_facts/temperature.html
url = "https://static.bc-edx.com/data/web/mars_facts/temperature.html"
browser.visit(url)
    
    
    Deliverable 2: Scrape and analyze Mars weather data, which exists in a table.
    # Create a Beautiful Soup object
# Create a Beautiful Soup object
html = browser.html
soup = BeautifulSoup(html, 'html.parser')

   # Extract all rows of data
    table = soup.find('table') 


# Part 1: Scrape Titles and Preview Text from Mars News 
Open the Jupyter Notebook in the starter code folder named part_1_mars_news.ipynb. You will work in this code as you follow the steps below to scrape the Mars News website.

    Use automated browsing to visit the Mars news site 

Links to an external site.. Inspect the page to identify which elements to scrape. 

Create a Beautiful Soup object and use it to extract text elements from the website.

Extract the titles and preview text of the news articles that you scraped. Store the scraping results in Python data structures as follows:

    Store each title-and-preview pair in a Python dictionary and, give each dictionary two keys: title and preview. 
 
  {'title': "NASA's MAVEN Observes Martian Light Show Caused by Major Solar Storm",
 'preview': "For the first time in its eight years orbiting Mars, NASA’s MAVEN mission witnessed two different types of ultraviolet aurorae simultaneously, the result of solar storms that began on Aug. 27."} 

  Store all the dictionaries in a Python list. 

  # Create an empty list to store the dictionaries
title_preview_list = []

  Print the list in your notebook.
# Print the list to confirm success
title_preview_list

 # Part 2: Scrape and Analyze Mars Weather Data 

 Open the Jupyter Notebook in the starter code folder named part_2_mars_weather.ipynb. You will work in this code as you follow the steps below to scrape and analyze Mars weather data.

    # Import relevant libraries
from splinter import Browser
from bs4 import BeautifulSoup
import matplotlib.pyplot as plt
import pandas as pd  

browser = Browser('chrome')

    Use automated browsing to visit the Mars Temperature Data Site 
 # Visit the website
# https://static.bc-edx.com/data/web/mars_facts/temperature.html
url = "https://static.bc-edx.com/data/web/mars_facts/temperature.html"
browser.visit(url)   

Links to an external site.. Inspect the page to identify which elements to scrape. Note that the URL is https://static.bc-edx.com/data/web/mars_facts/temperature.html.

# Create a Beautiful Soup object and use it to scrape the data in the HTML table. Note that this can also be achieved by using the Pandas read_html function. However, use Beautiful Soup here to continue sharpening your web scraping skills.

# Create a Beautiful Soup Object 
html = browser.html

soup = BeautifulSoup(html, 'html.parser') 

# Extract all rows of data
table = soup.find('table')



# Assemble the scraped data into a Pandas DataFrame. The columns should have the same headings as the table on the website. Here’s an explanation of the column headings:

    ### id: the identification number of a single transmission from the Curiosity rover
    terrestrial_date: the date on Earth
    sol: the number of elapsed sols (Martian days) since Curiosity landed on Mars
    ls: the solar longitude
    month: the Martian month
    min_temp: the minimum temperature, in Celsius, of a single Martian day (sol)
    pressure: The atmospheric pressure at Curiosity's location  

 # Create an empty list
headers = []
for th in table.find_all('th'):
    headers.append(th.text.strip())
# Extract table rows
rows = []
for tr in table.find_all('tr'):
    cells = tr.find_all('td')
    if len(cells) > 0:
        row = {}
        for i, cell in enumerate(cells):
            row[headers[i]] = cell.text.strip()
        rows.append(row)   



# Examine the data types that are currently associated with each column. If necessary, cast (or convert) the data to the appropriate datetime, int, or float data types.  

# Create a Pandas DataFrame by using the list of rows and a list of the column names
df = pd.DataFrame(rows)



# Analyze your dataset by using Pandas functions to answer the following questions:

    ## How many months exist on Mars? 
  # 1. How many months are there on Mars?
month = df.groupby('month')['month'].count()
month

    ## How many Martian (and not Earth) days worth of data exist in the scraped dataset?
  # 2. How many Martian days' worth of data are there?
martian_days = df['sol'].count()
martian_days
    
    ## What are the coldest and the warmest months on Mars (at the location of Curiosity)? To answer this question:
        Find the average minimum daily temperature for all of the months.
        # 3. What is the average low temperature by month?
min_temp_avg = df.groupby('month')['min_temp'].mean()
min_temp_avg    
        
         Plot the results as a bar chart.
          # Plot the average temperature by month
min_temp_avg.plot.bar(x='month', y='min_temp')

# Add labels 
plt.ylabel('Temperature in Celsius')
plt.show()

  ![Untitled](https://github.com/user-attachments/assets/835af7cc-e2e9-4dc5-ac16-01afaf1e5f07)


         
    
    ## Which months have the lowest and the highest atmospheric pressure on Mars? To answer this question:
        Find the average daily atmospheric pressure of all the months. 
        # Identify the coldest and hottest months in Curiosity's location
monthly_min_temp_sorted = min_temp_avg.sort_values()  

# Plot the average temperature by month
monthly_min_temp_sorted.plot(kind='bar') 

plt.xlabel('month')
plt.ylabel('Temperature in Celsius')
plt.show()
        
        Plot the results as a bar chart. 

        ![Untitled](https://github.com/user-attachments/assets/64be7ccb-d83f-4d5e-b06c-65935a44b381)

        



        
        
    ## About how many terrestrial (Earth) days exist in a Martian year? To answer this question: 
      Consider how many days elapse on Earth in the time that Mars circles the Sun once.
# conversion factor
earth_days_per_sol = 1.027
earth_days = 687 

# Calculate Sols
sols = earth_days / earth_days_per_sol
sols

# 5. How many terrestrial (earth) days are there in a Martian year?
df.plot(x='sol', y='min_temp', kind='line') 
# add labels 
plt.xlabel('Number of terrestrial days') 
plt.ylabel('Minimum temperature')
plt.show() 

![Untitled](https://github.com/user-attachments/assets/79ef0788-dce7-4855-a1a5-566511ebc3e3)


      # 4. Average pressure by Martian month
avg_pressure = df.groupby('month')['pressure'].mean()
avg_pressure

        # Plot the average temperature by month
monthly_avg_pressure_sorted.plot(kind='bar') 

#add labels
plt.xlabel('month')
plt.ylabel('Atmospheric Pressure')
plt.show()

![Untitled](https://github.com/user-attachments/assets/c3bf5740-b1ac-435f-9af0-143c31f1c0b8)



        Visually estimate the result by plotting the daily minimum temperature. 

      

Export the DataFrame to a CSV file.
# Write the data to a CSV
# Specify the CSV file name 
csv_file = 'mars_temp_data.csv' 

# Write the DataFrame to the CSV file 
df.to_csv(csv_file, index=False)

browser.quit()



