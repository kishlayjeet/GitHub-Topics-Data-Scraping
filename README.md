# Data extraction from the top repositories of featured topics on GitHub

Data extraction is a technique used to extract data from websites.
It involves fetching a web page and parsing its HTML or XML content to extract the data you are interested in.

#### To perform web scraping, you need the following:

- A website to scrape. You can choose any website that you have permission to scrape, but for this, we'll use a public website, GitHub.
- A web scraper tool. There are many tools available for web scraping, but for this, we'll use a Python library called `BeautifulSoup`.
- A programming language. In this case, we'll use Python for web scraping scripts, but you can use any language that you are comfortable with.

#### Here are the steps we'll follow:

- Data will be scraped from [Github topics](https://github.com/topics).
- We'll get a list of 100 topics. From each topic, we'll grab the topic's title, description, and URL.
- For topics, we'll create a CSV file in the following format:

| topic_title | topic_description                                                                            | topic_url                           |
| :---------- | :------------------------------------------------------------------------------------------- | :---------------------------------- |
| 3D          | 3D modeling is the process of virtually developing the surface and structure of a 3D object. | https://github.com/topics/3d        |
| Ajax        | Ajax is a technique for creating interactive web applications.                               | https://github.com/topics/ajax      |
| Algorithm   | Algorithms are self-contained sequences that carry out a variety of tasks.                   | https://github.com/topics/algorithm |
| Amp         | Amp is a non-blocking concurrency library for PHP.                                           | https://github.com/topics/amphp     |
| Android     | Android is an operating system built by Google designed for mobile devices.                  | https://github.com/topics/android   |

Then,

- For each topic, we'll get the top 20 repositories from the topic page.
- For each repository, we'll grab the username, repository name, stars, and repository URL.
- For each topic, we'll create a CSV file in the following format:

| username  | repo_name         | stars | repo_url                                    |
| :-------- | :---------------- | :---- | :------------------------------------------ |
| mrdoob    | three.js          | 87300 | https://github.com/mrdoob/three.js          |
| libgdx    | libgdx            | 20800 | https://github.com/libgdx/libgdx            |
| pmndrs    | react-three-fiber | 20600 | https://github.com/pmndrs/react-three-fiber |
| BabylonJS | Babylon.js        | 18900 | https://github.com/BabylonJS/Babylon.js     |
| ssloy     | tinyrenderer      | 15400 | https://github.com/ssloy/tinyrenderer       |

## Scrape the list of topics from Github.

Explain how we'll do that:

- We'll use [requests](https://pypi.org/project/requests/) to download the page.
- and we'll use [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) to parse and extract information from the downloaded page.
- then we'll convert to a [Pandas](https://pandas.pydata.org) DataFrame.

Let's write a function to download the page.

```python
  import requests
  from bs4 import BeautifulSoup

  def get_page_content(page_url):
    response = requests.get(page_url)
    if response.status_code != 200:
      raise Exception(f'Failed to load page {page_url}')
    page_doc = BeautifulSoup(response.text, 'html.parser')
    return page_doc
```

- In the above code, first we imported `requests` and used `BeautifulSoup` to download and extract the information from the downloaded page.
- then we wrote a function, `get_page_content()`, to retrieve the content of the page where we used the `requests.get()` function to get the page content.
- then we checked if the response we got was successful, and then we used `BeautifulSoup` to extract the page's content.

#### When we get the content of the page, then we have to scrape the data that we want. So let's write some other functions to parse information from the page.

```python
  def get_topic_title(doc):
    topic_title_tag = doc.find_all('p', {'class':'f3 lh-condensed mb-0 mt-1 Link--primary'})
    topic_title = []
    for item in topic_title_tag:
      topic_title.append(item.text.strip())
    return topic_title
```

- In the above code, we created the function `get_topic_title()` to get the list of titles and insert them into a list.
- To get topic titles, we picked `p` tags with the `class` attribute. ![](https://imgur.com/fz9lCNY.png)

Similarly, we have defined functions to get descriptions and URLs too.

```python
  def get_topic_desc(doc):
    topic_desc_tag = doc.find_all('p', {'class':'f5 color-fg-muted mb-0 mt-1'})
    topic_desc = []
    for item in topic_desc_tag:
      topic_desc.append(item.text.strip())
    return topic_desc
```

```python
  def get_topic_url(doc):
    topic_url_tag = doc.find_all('a', {'class':'no-underline flex-1 d-flex flex-column'})
    base_url = 'https://github.com'
    topic_url = []
    for item in topic_url_tag:
      topic_url.append(base_url + item['href'])
    return topic_url
```

- In the `get_topic_url()` function, we also added `base_url` to get the exact url of the topic.

#### After getting all the information about topics, we'll create another function that combines all the information and returns a `DataFrame`.

```python
  import pandas as pd

  def scrape_topics_info(page_doc):
    topics_dict = {
        'topic_title': get_topic_title(page_doc),
        'topic_desc': get_topic_desc(page_doc),
        'topic_url': get_topic_url(page_doc)
    }
    return pd.DataFrame(topics_dict)
```

- In the above code, first we imported `Pandas` for creating a `DataFrame` and then defined the function `scrape_topics_info()`.

#### After getting a `DataFrame` of the combined information, we'll create a function to convert the `DataFrame` into `CSV`.

```python
  def import_to_csv(data_frame):
    data_frame.to_csv('./topics.csv', index = None)
```

But here a problem occurred when we wanted to scrape the top 100 topics' information.

- Problem: On each page of Github, there are only 20 or 30 topics, and we want 100.
- Approach: We have to make a function that returns the URL of the next page for more topics.

```python
  def get_pages(page):
    reqUrl = f"https://github.com/topics?page={page}"
    return reqUrl
```

Now the problem is solved, but we'll have to create a function that filters the top 100 topics.

```python
  def update_and_filter_data(new_df):
    re_index = [*range(100, len(new_df))]
    update_df = new_df.drop(re_index)
    import_to_csv(update_df)
```

- The `update_and_filter_data()` function will filter the top 100 topics and update the `CSV`.
  As a result, we scraped all of the information for 100 topics from Github, now we have to scrape the top 20 repositories from a topic.

## Get the top 20 repositories from the topic page.

Explain how we'll do it:

- We'll access all the topics through the URL that we stored in `CSV`.
- then we'll use `requests` to download each topic's page.
- then we'll use `BeautifulSoup` to parse and extract information from the downloaded page.
- then convert to a `Pandas` DataFrame and then convert into `CSV`.

Let's write a function to access each topic page.

```python
  def access_topic_page_url(df, topic_num):
    return df['topic_url'][topic_num]
```

- In the above code, we defined the function `access_topic_page_url()`, which returns each topic page URL.
- and then we called `get_page_content()`, which we defined earlier along with a URL to download page content.

#### After that, we'll have to create a function that scrapes all the top 20 repositories from each topic and returns a `DataFrame`.

```python
  def get_topic_repos(topic_doc):
    repo_tag = topic_doc.find_all('h3', {'class':'f3 color-fg-muted text-normal lh-condensed'})
    star_tag = topic_doc.find_all('span', {'class':'Counter js-social-count'})

    topic_repos_dict = {
        'username': [],
        'repo_name': [],
        'stars': [],
        'repo_url': []
        }

    for i in range(len(repo_tag)):
      repo_info = get_repo_info(repo_tag[i], star_tag[i])
      topic_repos_dict['username'].append(repo_info[0])
      topic_repos_dict['repo_name'].append(repo_info[1])
      topic_repos_dict['stars'].append(repo_info[2])
      topic_repos_dict['repo_url'].append(repo_info[3])
    return pd.DataFrame(topic_repos_dict)
```

- To get the repository name, username, and repo url, we picked `h3` tags with the `class` attribute. ![](https://imgur.com/SeI87et.png)

- Similarly, we selected `span` tags with the `class` for stars. ![](https://imgur.com/wYH4jSP.png)

- Then we created a dictionary, `topic_repos_dict`, of lists for storing all the information about repositories.
- Then we created another function, `get_repo_info()`, for extracting all the information about a repository and returning all the information.

```python
  def get_repo_info(repo_tag, star_tag):
    base_url = 'https://github.com'
    a_tag = repo_tag.find_all('a')
    username = a_tag[0].text.strip()
    repo_name = a_tag[1].text.strip()
    stars = parse_star_count(star_tag.text.strip())
    repo_url = base_url + a_tag[1]['href']
    return username, repo_name, stars, repo_url
```

- The star count of repositories is in a different format. ![](https://imgur.com/MxsQrqr.png)

- Because of that, we created another function, `parse_star_count()`, to parse the star count, and called in the `get_repo_info()` function.

```python
  def parse_star_count(stars_str):
    stars_str = stars_str.strip()
    if stars_str[-1] == 'k':
      return int(float(stars_str[:-1])*1000)
    else:
      return int(stars_str)
```

#### When all of the tasks are completed and `get_topic_repos()` returns the `DataFrame`, we'll create another function to convert the `DataFrame` to `CSV`. 

```python
  def import_topic_repos_to_csv(df, topic):
    df.to_csv(f'./topics/repos/{topic}.csv', index=None)
```

##### Now that we've written all of the necessary functions, let's use them to scrape the data. 

## Let's Scrape

```python
  temp_df = pd.DataFrame()
  for page in range(1, 6):
    if len(temp_df) < 100:
      url = get_pages(page)
      page_doc = get_page_content(url)
      df = scrape_topics_info(page_doc)
      temp_df = temp_df.append(df)
    else:
      break
  import_to_csv(temp_df)
  new_df = pd.read_csv('./topics/topics.csv')
  update_and_filter_data(new_df)
```

- Initializes first an empty `DataFrame` called `temp_df` using the pandas library.
- iterates over a range of pages (from 1 to 5) and extracts all the information about topics.
- uses the `import_to_csv()` function to export the data into a CSV file called `topics.csv`.

```python
  new_df = pd.read_csv('./topics/topics.csv')
  for page in range(len(new_df)):
    url = access_topic_page_url(new_df, page)
    doc = get_page_content(url)
    df = get_topic_repos(doc)
    import_topic_repos_to_csv(df, new_df['topic_title'][page])
```

- Reads the `topics.csv` file into a new DataFrame called `new_df` using the pandas library.
- Iterates over the rows in `new_df` and extracts the top 20 repositories from each topic.
- uses the `import_topic_repos_to_csv()` function to export the data into a CSV file named after the topic.

You can also get code snippet from [here]().

## Authors

Hi there! I'm [Kishlay](https://www.github.com/kishlayjeet), and I've created a project that uses web scraping to extract data from GitHub.
The project is written in Python and makes use of the BeautifulSoup library to parse HTML content from the website.
The resulting data is saved in a CSV file that includes a list of the top 100 topics from GitHub as well as the top 20 repositories for each topic.
If you have any feedback or suggestions for how to improve the project, please let me know.
And if you find it useful, please consider giving it a star.

Thank you!

## Feedback

If you have any feedback, please reach out to me at contact.kishlayjeet@gmail.com
