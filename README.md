# Data Extraction from the Top Repositories of Featured Topics on GitHub

Data extraction is a crucial technique in web scraping that involves fetching a web page and parsing its HTML or XML content to extract relevant data. This project focuses on data extraction from GitHub, a popular platform for software development. The project extracts information on the top 100 topics from GitHub, including the topic's title, description, and URL. Additionally, the top 20 repositories for each topic are extracted, including the username, repository name, number of stars, and repository URL.

## Prerequisites

To perform web scraping, the following is required:

- **A website to scrape:** In this case, we'll use GitHub as the source of information.
- **A web scraper tool:** For this project, we'll use the Python library, BeautifulSoup.
- **A programming language:** Python will be used in this project, but you can use any language that you're comfortable with.

## Data Extraction Steps

#### Step 1: Scrape the list of topics from GitHub

To extract the list of topics from GitHub, we'll follow these steps:

1. Download the page using the `requests` library.
2. Parse the downloaded page using the `BeautifulSoup` library.
3. Convert the extracted information into a `Pandas` DataFrame.

#### Step 2: Get the top 20 repositories from the topic page

To extract the top 20 repositories for each topic, we'll follow these steps:

1. Access each topic through the URL stored in the CSV file created in step 1.
2. Download each topic's page using the `requests` library.
3. Parse the downloaded page using the `BeautifulSoup` library.
4. Convert the extracted information into a `Pandas` DataFrame.
5. Save the information as a CSV file.

## Author

Hi! I'm Kishlay, the creator of this project. I used web scraping and data extraction techniques to gather information from GitHub and save it in a CSV file. The project is written in Python and uses the BeautifulSoup library to parse HTML content from the website.

## Feedback

If you have any feedback or suggestions, please don't hesitate to reach out to me at contact.kishlayjeet@gmail.com.
