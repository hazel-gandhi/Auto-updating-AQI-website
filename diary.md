In order to scrape the Air Quality Index (AQI) of India, I chose to use the daily data put out by India's Central Pollution Control Board. I had two options: 
(a) using the website's dashboard 
(b) scraping the daily PDFs they uploaded on their website with the AQI for every city.
I chose to go with option (a) because at that point, I did not know how to scrape PDFs and I figured I'd use Playwright to scrape the dashboard and all its dropdowns, push it all in a pandas dataframe and set up Github Actions to make it update every day. I encountered a few issues when trying to do this: 
-The dropdowns were impossible to loop through because they were in the form of a string. If they were integers, i could just make a range out of them and loop through that range. 
-I could solve this by hardcoding a list of all cities and scraping the AQIs individually and then adding those to my dataframe. 
-But then I realized that just making a list of all cities in the dropdown wasn't enough. The website was designed in a way where I had to select the city, state, and even railway stations for the AQI page to load. This meant that there were far more buttons to click and too many options to go through to get the AQI of a single city. And there are 250+ cities in the database. All these reasons led me to give up on this method and try option (b).

Scraping the PDFs:
The website has an archive section where they upload daily PDFs of the average AQI of a city in the last 24 hours. 
I scraped the PDF for the latest day and then started setting up Github Actions. 
I quickly realized that in the tutorial, Soma was working with a URL that updated daily and was scraping that URL. But I had to scrape the new PDF that appeared on the updated URL every day. So just writing a scraper for a pdf would not be enough. 
To solve this, I used beautifulsoup to first go to the URL, look for the tag where the PDF is uploaded and scrape that. I then used the datetime library to name the file according to the date of the document and then merged this with my PDF scraper. 
This helped me grab the latest pdf, rename it according to the date so I know which PDF is being scraped, and then make a dataframe out of it. 
I did not know about the datetime library before so that was something new and useful that I got to learn. 

Setting up Github Actions:
After this, I set up github actions and added extra imports in my yml file including datetime and Pillow, which I learnt with the help of a TA that it was installed along with img2table.
After getting multiple errors due to missing imports, I was finally able to make my code run.

Making the map:
Tried to find a geojson file that had the coordinates of the Indian cities in my database. Could not find any.
Used the Google Geocoding API (with some help from ChatGPT) to get the coordinates on my own and merged that with my original dataframe.
Uploaded the new dataframe to Datawrapper. 
Adjusted the range of AQI with corresponding colors, set up the width of the map, made some font changes to the HTML. 

Issues faced, need resolving:
1. Could not adjust the height of the map. For this, I tried changing the height inside the embed code, the CSS, and even within Datawrapper settings, but nothing worked. Right now, my map places the main content on the right hand side and not in the center like I wanted. That is one issue I wanted to resolve
2. Would have liked to add a line on top that specifies the date of the AQI, something like "AQI as on December 15, 2024". Tried writing my scraper inside VS Code and running it but it wasn't working. 
