## Part 1: Data Scraping

The purpose of this section is to scrape the red light camera locations from the website and save them into a dataframe.


```python
# Standard imports
import numpy as np
import pandas as pd

# For web data scraping
import requests
from bs4 import BeautifulSoup

# For adding delays so that we don't spam requests
import time
```

The data is avaliable at city of Toronto's website, however, scraping the list is disallowed. The same list is found in insurancexperts.ca, which will be used for scraping.

Add `robots.txt` at the end of website url to check permissions


```python
# Send the get request
response = requests.get('https://insurancexperts.ca/red-light-cameras-toronto-list-2019/')

# Turn the response.content into a Beautiful Soup object
soup = BeautifulSoup(response.content)
```


```python
rlc_locations = []

# Pull out all the HTML elements from the list and save to 'rlc_locations'
ol = soup.ol

for li in ol.findAll('li'):
    rlc_locations.append(li.text)

# Check the list length
len(rlc_locations)
```




    148




```python
# Save the formatted locations to a dataframe
rlc_df = pd.DataFrame({'location': rlc_locations})
```


```python
# Check the dataframe
rlc_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADELAIDE ST E   PARLIAMENT ST</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ALBION RD   KIPLING AVE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALBION RD   SILVERSTONE DR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AVENUE RD   LAWRENCE AVE W</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BATHURST ST   DAVENPORT RD</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Replace special characters to &
rlc_df.replace('\xa0 \xa0', '&',regex=True,inplace=True)
```


```python
# Check the final dataframe
rlc_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ADELAIDE ST E&amp;PARLIAMENT ST</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ALBION RD&amp;KIPLING AVE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ALBION RD&amp;SILVERSTONE DR</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AVENUE RD&amp;LAWRENCE AVE W</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BATHURST ST&amp;DAVENPORT RD</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Save the dataframe to csv file for later use
rlc_df.to_csv('data/rlc.csv', encoding='utf-8', index=False)
```


```python

```
