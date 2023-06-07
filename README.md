# Data Extraction from Top Repositories of Featured Topics on GitHub

This project focuses on web scraping and data extraction techniques to retrieve valuable information from GitHub. Specifically, we will scrape the top repositories associated with featured topics on GitHub.

## Prerequisites

To perform web scraping, you will need:

- **A website to scrape:** In this project, we will use GitHub as our data source.
- **A web scraping tool:** We will use the Python library BeautifulSoup for web scraping.
- **A programming language:** This project is implemented in Python, but you can use any programming language of your choice.

## Data Extraction Steps

### Step 1: Scrape the List of Topics from GitHub

To extract the list of featured topics from GitHub, follow these steps:

1. Download the webpage using the `requests` library.
2. Parse the downloaded webpage using BeautifulSoup.
3. Convert the extracted information into a Pandas DataFrame.

Example DataFrame:

| topic_title | topic_description                                                                            | topic_url                           |
| :---------- | :------------------------------------------------------------------------------------------- | :---------------------------------- |
| 3D          | 3D modeling is the process of virtually developing the surface and structure of a 3D object. | https://github.com/topics/3d        |
| Ajax        | Ajax is a technique for creating interactive web applications.                               | https://github.com/topics/ajax      |
| Algorithm   | Algorithms are self-contained sequences that carry out a variety of tasks.                   | https://github.com/topics/algorithm |

### Step 2: Get the Top 20 Repositories for Each Topic

To extract the top 20 repositories for each topic, follow these steps:

1. Access each topic's URL stored in the CSV file created in Step 1.
2. Download the webpage for each topic using the `requests` library.
3. Parse the downloaded webpage using BeautifulSoup.
4. Convert the extracted information into a Pandas DataFrame.
5. Save the extracted information as a CSV file.

Example DataFrame:

| username | repo_name         | stars | repo_url                                    |
| :------- | :---------------- | :---- | :------------------------------------------ |
| mrdoob   | three.js          | 87300 | https://github.com/mrdoob/three.js          |
| libgdx   | libgdx            | 20800 | https://github.com/libgdx/libgdx            |
| pmndrs   | react-three-fiber | 20600 | https://github.com/pmndrs/react-three-fiber |

## Author

Hi! I'm Kishlay, the creator of this project. I utilized web scraping and data extraction techniques to gather information from GitHub and save it in a CSV file. The project is implemented in Python and uses the BeautifulSoup library to parse HTML content from the website.

## Feedback

If you have any feedback or suggestions, please don't hesitate to reach out to me at contact.kishlayjeet@gmail.com.
