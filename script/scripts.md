# Web Scraping — Wikipedia Revenue Table (Requests + BeautifulSoup)

This repository documents a minimal web-scraping workflow used to fetch a Wikipedia page, parse the HTML, locate the relevant table, and extract header fields for downstream tabular processing.

---

## Tech Stack

- Python
- `requests` (HTTP GET)
- `bs4` / `BeautifulSoup` (HTML parsing)

---

## Script Syntax Extracted From Notebook (With Comments)

> The code below is assembled from the notebook cells in execution order.
> Inline comments explain *why* each step is performed.

```python
# 1) Import dependencies
# Why: requests is used to fetch HTML over HTTP; BeautifulSoup is used to parse and query the HTML structure.
from bs4 import BeautifulSoup
import requests


# 2) Define the target Wikipedia URL
# Why: this is the page that contains the "largest companies by revenue" table.
url = "https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue"


# 3) Make an initial HTTP request
# Why: this tests basic connectivity and returns an HTTP response object.
page = requests.get(url)
print(page)  # Why: printing the response helps confirm status (e.g., <Response [200]> for success).


# 4) Re-define the URL (same target)
# Why: the notebook repeats the URL assignment before retrying the request with headers.
url = "https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue"


# 5) Add request headers (User-Agent)
# Why: some sites return different content or block requests that look like bots;
#      setting a User-Agent can improve reliability.
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
}


# 6) Make the HTTP request again using headers
# Why: this is the “browser-like” request intended to reduce blocking / content gating.
page = requests.get(url, headers=headers)
print(page)  # Why: verify the second request also succeeded.


# 7) Parse the HTML into a BeautifulSoup object
# Why: page.text is raw HTML; BeautifulSoup converts it into a searchable parse tree.
soup = BeautifulSoup(page.text, "html")
print(soup)  # Why: printing soup allows inspection of the full HTML output during debugging.


# 8) Find the first table element
# Why: quick check that tables exist on the page and that parsing worked.
soup.find("table")


# 9) Find all tables on the page
# Why: Wikipedia pages often contain multiple tables; this returns a list for indexing/selection.
soup.find_all("table")


# 10) Select the table at index 1
# Why: the notebook uses index-based selection to isolate the intended wikitable instance.
soup.find_all("table")[1]


# 11) Select a table by its class attribute
# Why: Wikipedia tables commonly use class="wikitable sortable"; filtering by class is more robust than index.
soup.find("table", class_="wikitable sortable")


# 12) Assign a table object using index-based selection (index 0 in the notebook)
# Why: the table is stored in a variable so it can be reused for header/row extraction.
table = soup.find_all("table")[0]
print(table)  # Why: confirm the extracted table HTML matches the expected revenue table.


# 13) Find all header cells (<th>) in the entire page
# Why: this is a global check to see how many header elements exist overall.
soup.find_all("th")


# 14) Find header cells (<th>) within the selected table only
# Why: revenue table headers are contained in <th> tags; isolating to the chosen table avoids unrelated headers.
world_titles = table.find_all("th")

