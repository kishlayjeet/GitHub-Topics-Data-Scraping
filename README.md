# Data Extraction from Top Repositories of Featured Topics on GitHub

Data extraction is a powerful technique that allows you to retrieve valuable data from websites. In this project, we will perform web scraping on GitHub, the popular code hosting platform, to extract information about popular topics and the top repositories associated with them.

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

| topic_title | topic_description                                                                            | topic_url                           |
| :---------- | :------------------------------------------------------------------------------------------- | :---------------------------------- |
| 3D          | 3D modeling is the process of virtually developing the surface and structure of a 3D object. | https://github.com/topics/3d        |
| Ajax        | Ajax is a technique for creating interactive web applications.                               | https://github.com/topics/ajax      |
| Algorithm   | Algorithms are self-contained sequences that carry out a variety of tasks.                   | https://github.com/topics/algorithm |

#### Step 2: Get the top 20 repositories from the topic page

To extract the top 20 repositories for each topic, we'll follow these steps:

1. Access each topic through the URL stored in the CSV file created in step 1.
2. Download each topic's page using the `requests` library.
3. Parse the downloaded page using the `BeautifulSoup` library.
4. Convert the extracted information into a `Pandas` DataFrame.
5. Save the information as a CSV file.

| username | repo_name         | stars | repo_url                                    |
| :------- | :---------------- | :---- | :------------------------------------------ |
| mrdoob   | three.js          | 87300 | https://github.com/mrdoob/three.js          |
| libgdx   | libgdx            | 20800 | https://github.com/libgdx/libgdx            |
| pmndrs   | react-three-fiber | 20600 | https://github.com/pmndrs/react-three-fiber |

## Author

Hi! I'm Kishlay, the creator of this project. I used web scraping and data extraction techniques to gather information from GitHub and save it in a CSV file. The project is written in Python and uses the BeautifulSoup library to parse HTML content from the website.

## Feedback

If you have any feedback or suggestions, please don't hesitate to reach out to me at contact.kishlayjeet@gmail.com.
