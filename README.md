# Technical Build Log — Web Scraping Pipeline for Revenue Data

This project documentsthe exact sequence of steps required to scrape, structure, and export company revenue data from a public web source. 

---

## Data Connection and Source Configuration

In a python library, 
Import the `requests` library to handle HTTP connections and import `BeautifulSoup` from the `bs4` package to parse HTML content.  
Define the variable `url` and assign it the Wikipedia page titled *List of largest companies in the United States by revenue*.  
Execute `requests.get(url)` to retrieve the response object containing the raw HTML payload.  
Instantiate a `BeautifulSoup` object using `page.text` as the input and specify `'html.parser'` as the parsing engine.

---

## Field Preparation and Transformations

Inspect the document object model to identify the `<table>` element containing the company rankings and revenue data.  
Execute `soup.find_all('table')` and select the object at index `1`, corresponding to the table with the HTML class `wikitable sortable`.  
Extract column headers by calling `table.find_all('th')` on the identified table object.  
Apply the list comprehension `[title.text.strip() for title in world_titles]` to remove newline characters and surrounding whitespace from the header text.

Extract all table rows using `table.find_all('tr')`.  
Apply slicing logic `column_data[1:]` to exclude the empty header row and prevent column alignment errors during iteration.

---

## Calculated Fields and Data Structure (Pandas)

Import the `pandas` library as `pd`.  
Instantiate an empty DataFrame named `df` and pass the cleaned header list `world_table_titles` to the `columns` parameter.  
Iterate over each row element using a `for` loop and extract all `<td>` elements via `row.find_all('td')`.  
Apply the transformation `data.text.strip()` to each cell to remove whitespace and newline characters.
