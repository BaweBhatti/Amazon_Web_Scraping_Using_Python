# Amazon_Web_Scraping_Using_Python
## 🔍Project Overview:
   - Developed a project on Web scraping using Python on Amazon website. The primary objective of this project is to collect and parse raw data from the website and transform into structured
     form( in this case, an excel file).
     

## 🔮Tools and Libraries:
   - Used **Jupyter Notebook** to run Python code
   - Used **BeautifulSoup** module to extract data from Amazon's website. This library is used for parsing HTML and XML documents.
   - Manipulated and cleaned data using Pythons **Panda** library
   
## 🕸️Target Website Information:
   - Scraped information of Amazon.ca website. The website structure is matrix or more precisely webbed website structure. It gives total freedom to customers to browse.
     So, it has a main page along with basic parent pages and their child pages but the order in which customers find them is largely irrelevant.
   - Wanted to chooce a high demand e-commerce website for this static project to showcase my skills.

## 💭Data Extraction Logic:
   1. Firstly started by importing necessary modules. To perform web scraping, imported The Beautiful Soup package which is used to extract data from html files. The library's name is bs4 which stands for Beautiful Soup, version 4.
  ```python
         #import libraries
          from bs4 import BeautifulSoup
          import requests
          import pandas as pd
          import numpy as np
  ```
   2. Creating functions to extract Product Title, Price, Rating and Number of Review.
     First Python code defines a function called **get_title** that takes a BeautifulSoup 'soup' object as input. 
     Breakdown of what the code does:
        - ***try-except block***: The code is wrapped in a try-except block for error handling.
        - ***Finding the Title***: Inside the try block, the code attempts to find an HTML element with the tag <span> and the attribute id set to 'productTitle' within the provided soup object. The find method 
             of the soup object used to extract the title of the product from the webpage.
        - ***Extracting Title Text***: If the title element is found, the code extracts the text content of the title element using the .text attribute. This will give us a NavigableString 
             object, which represents the text within the HTML element.
        - ***Stripping Whitespace***: Used '.strip()' to remove leading or trailing whitespace characters from the extracted title text.
        - ***Error Handling***: If any errors occur during the execution of the code inside the try block (for instance, if the specified element is not found), the except block will catch the AttributeError and 
             set the title_string variable to an empty string ("").
        - ***Return Value***: Finally, the function returns the extracted and cleaned title string (title_string), which will either contain the title of the product or an empty string if no title is found or if 
             an error occurs.

        *Rest of the functions created follows the same code to extract Price, Rating and Number of Review.*
          
      ```python
              #Function to extract Product Title
               def get_title(soup):
         
                  try:
                      # Outer Tag Object
                      title = soup.find("span", attrs={"id":'productTitle'})
                      
                      # Inner NavigableString Object
                      title_value = title.text
              
                      # Title as a string value
                      title_string = title_value.strip()
              
                  except AttributeError:
                      title_string = ""
              
                  return title_string
      
              #Function to extract Product Price
                def get_price(soup):
                
                    try:
                        price = soup.find("span", attrs={'class':'a-price aok-align-center'}).string.strip()
                
                    except AttributeError:
                            price = ""
                
                    return price
      
              #Function to extract Product Rating
                def get_rating(soup):
                
                    try:
                        rating = soup.find("i", attrs={'class':'a-icon a-icon-star a-star-4-5 cm-cr-review-stars-spacing-big'}).string.strip()
                    
                    except AttributeError:
                        try:
                            rating = soup.find("span", attrs={'class':'a-icon-alt'}).string.strip()
                        except:
                            rating = ""	
                
                    return rating
      
              #Function to extract Number of User Reviews
                def get_review_count(soup):
                  try:
                      review_count = soup.find("span", attrs={'id':'acrCustomerReviewText'}).string.strip()
              
                  except AttributeError:
                      review_count = ""	
              
                  return review_count
      ```
        
  3. Breakdown of the code below:
      - ***User Agent and Headers***: It defines a dictionary named HEADERS containing user-agent information. 
      - ***Webpage URL***: It defines a URL variable (URL) containing the link to an Amazon product page.
      - ***HTTP Request***: It uses the requests.get() function to send an HTTP GET request to the specified URL. It includes the HEADERS dictionary as part of the request to mimic a web browser. The response 
           from the server is stored in the webpage variable.
      - ***Soup Object Creation***: It creates a BeautifulSoup object named soup from the content of the webpage response (webpage.content). This object will be used to parse the HTML content of the webpage and 
           extract information from it.
        
        Overall, this code segment prepares to scrape data from the specified Amazon product page by sending an HTTP request with customized headers and creating a BeautifulSoup object to parse the HTML content 
        of the webpage.
         
        ```python
              #added user agent 
                HEADERS = ({"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36", 'Accept-Language': 'en-US, en;q=0.5'})
      
              #The webpage URL
                URL='https://www.amazon.ca/Amazon-Essentials-Regular-Fit-Long-Sleeve-Mini-Gingham/dp/B06XW9QH4B/ref=sr_1_1_ffob_sspa? 
                crid=3HHVEZYJZ72LJ&dib=eyJ2IjoiMSJ9.efbm2bk6bX6pQFeJbNtuq5SNR_8YQbcIAe2LQSKqaoSqdcWgjaFsoRv4rk4IleTvnz6_PcltU3vwE1zz0ZCeKWkokkM7FxQbLoljKGGga1H_YQhTA3YVIIwi_CN38- 
                C3rJqlXjyqEcCrqG_rS4hgUN0DSKSCGI_y- 
                4eMEqHk64Mduq9qhwfdUjB3JQFkTNmkXRUp2yT1zT5wMN51FhI4kdOGP0OhYmsdxOk0AD2msmcsuVXEoyzhj6caP6hZTKzDQTFboACzC4ONs5xJR05WD0KOUh47iN57972GyjTSnZk.QqxFhlxSq2yzQDoP16v71L___- 
                Q5V6muW8napofNl40&dib_tag=se&keywords=shirt&qid=1713897973&sprefix=shirt%2Caps%2C202&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1'
                
                #HTTP Request
                webpage = requests.get(URL, headers=HEADERS)
                
                #Soup Object containing all data
                soup = BeautifulSoup(webpage.content, "html.parser")
        ```
   4. Then wrote code to scrape product links from Amazon page and initialize an empty dictionary for storing product information. Here's what each part does:
        - ***Fetching Links***: It uses the find_all() method of the BeautifulSoup soup object to find all <a> tags with specific attributes. These are typically the tags containing product links on Amazon 
             product pages. The result is stored in the links variable as a list of Tag objects.
        - ***Initializing an Empty List***: It initializes an empty list named links_list. This list will be used to store the extracted links.
        - ***Extracting Links***: It iterates over each Tag object in the links list and extracts the value of the 'href' attribute using the get() method. The extracted links are then appended to the links_list 
             list.
        - ***Initializing a Dictionary***: It initializes an empty dictionary named d with keys 'title', 'price', 'rating', and 'reviews'. This dictionary will be used to store product information such as title, 
          price, rating, and reviews.

        Overall, this code sets up the framework for scraping product links from an Amazon page and storing them in a list, as well as initializing an empty dictionary for storing product information.

      ```python
         #Fetch links as List of Tag Objects
          links = soup.find_all("a", attrs={'class':'a-link-normal s-underline-text s-underline-link-text s-link-style a-text-normal'})
          
           Store the links
          links_list = []
          
          # Loop for extracting links from Tag Objects
          for link in links:
              links_list.append(link.get('href'))
          
          d = {"title":[], "price":[], "rating":[], "reviews":[]}
      ```

   6. These function calls seem to be intended to populate the dictionary 'd' with product information extracted from the BeautifulSoup object soup. Here's what each function call does:
      - ***get_title(soup)***: This function call invokes the get_title function defined earlier, passing the BeautifulSoup object soup as an argument. It retrieves the title of the product from the webpage 
           using the specified HTML tag and attribute. The title is then appended to the list associated with the 'title' key in the dictionary d.
        
      *Other 3 function calls works the same as get_title*

      These function calls collectively aim to gather necessary product information (title, price, rating, and review count) from the webpage and store it in the dictionary d.
      ```python
                # Function calls to display all necessary product information
                  d['title'].append(get_title(soup))
                  d['price'].append(get_price(soup))
                  d['rating'].append(get_rating(soup))
                  d['reviews'].append(get_review_count(soup))
      ```

  7. This portion of the code involves processing the collected product information and saving it to a CSV file named "amazon_data.csv". Here's what each part does:
      - ***Creating DataFrame***: It creates a Pandas DataFrame named amazon_df from the dictionary d, where each key in the dictionary corresponds to a column in the DataFrame.
      - ***Replacing Empty Titles with NaN***: It replaces empty strings in the 'title' column of the DataFrame with NaN (Not a Number) using the replace() method from Pandas. This is done to handle cases where 
           the title information couldn't be retrieved.
      - ***Dropping Rows with NaN Titles***: It drops rows from the DataFrame where the 'title' column contains NaN values using the dropna() method. This ensures that only rows with valid title information are 
           retained in the DataFrame.
       - ***Exporting DataFrame to CSV***: It exports the cleaned DataFrame to a CSV file named "amazon_data.csv" using the to_csv() method from Pandas. The parameter header=True specifies that the column names 
            should be included as the first row in the CSV file, and index=False specifies that row indices should not be included in the CSV file.
           
     Overall, this code segment processes the collected product information, cleans the DataFrame by removing rows with missing title information, and saves the cleaned DataFrame to a CSV file for further 
     analysis or storage.
     ```python
         amazon_df = pd.DataFrame.from_dict(d)
         amazon_df['title'].replace('', np.nan, inplace=True)
         amazon_df = amazon_df.dropna(subset=['title'])
          amazon_df.to_csv("amazon_data.csv", header=True, index=False)
     ```

  8. Finally, displaying the dataframe
     ```python
     amazon_df
     ```

## 👩‍💻Closing Thoughts
It was a fun yet challenging project, where I have learned how to scrape data from a website using the requests library and BeautifulSoup library. Also learned how to store the scraped data in a csv file.




           
