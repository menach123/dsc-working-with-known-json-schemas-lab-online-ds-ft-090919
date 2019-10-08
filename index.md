
# Working with Known JSON Schemas - Lab

## Introduction
In this lab, you'll practice working with JSON files whose schema you know beforehand.

## Objectives
You will be able to:
* Read JSON Documentation Schemas and translate into code
* Extract data from known JSON schemas
* Write data to predefined JSON schemas

## Reading a JSON Schema

Here's the JSON schema provided for a section of the NY Times API:
<img src="images/nytimes_movie_schema.png" width=500>

or a fully expanded view:

<img src="images/nytimes_movie_schema_detailed.png" width=500>

You can more about the documentation [here](https://developer.nytimes.com/docs/movie-reviews-api/1/routes/reviews/%7Btype%7D.json/get).

You can see that the master structure is a dictionary and has a key named 'response'. This is also a dictionary and has two keys: 'data' and 'meta'. As you continue to examine the schema hierarchy, you'll notice the vast majority, in this case, are dictionaries. 

## Loading the Data File

Start by importing the json file. The sample response from the api is stored in a file **ny_times_movies.json**


```python
import json
import pandas as pd

with open("ny_times_movies.json") as f:
    d = json.load(f)
```


```python
def dict_method_to_list(method):
    return list(map(lambda x : x, method))

#### JSON Key chec, outputing names and file types
def key_check(data):
    
    for level in data.keys():
        
        print(level, type(data[level]))
        if type(data[level]) == dict:
            print('-------',level,'dict')
            
            key_check(data[level])
        if  type(data[level]) == list:
            print('-------', level, 'list', type(data[level][0]), len(data[level]), len(data[level][0]))




```

## Loading Specific Data

Create a DataFrame of the major data container within the json file, listed under the 'results' heading in the schema above.


```python
key_check(d)
```

    status <class 'str'>
    copyright <class 'str'>
    has_more <class 'bool'>
    num_results <class 'int'>
    results <class 'list'>
    ------- results list <class 'dict'> 20 11
    


```python
df = pd.DataFrame(d['results'])
df.head()
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
      <th>byline</th>
      <th>critics_pick</th>
      <th>date_updated</th>
      <th>display_title</th>
      <th>headline</th>
      <th>link</th>
      <th>mpaa_rating</th>
      <th>multimedia</th>
      <th>opening_date</th>
      <th>publication_date</th>
      <th>summary_short</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A.O. SCOTT</td>
      <td>1</td>
      <td>2018-10-17 02:44:23</td>
      <td>Can You Ever Forgive Me</td>
      <td>Review: Melissa McCarthy Is Criminally Good in...</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td>R</td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>2018-10-19</td>
      <td>2018-10-16</td>
      <td>Marielle Heller directs a true story of litera...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>BEN KENIGSBERG</td>
      <td>1</td>
      <td>2018-10-16 11:04:03</td>
      <td>Charm City</td>
      <td>Review: â€˜Charm Cityâ€™ Vividly Captures the ...</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td></td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>2018-04-22</td>
      <td>2018-10-16</td>
      <td>Marilyn Nessâ€™s documentary is dedicated to t...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>GLENN KENNY</td>
      <td>1</td>
      <td>2018-10-16 11:04:04</td>
      <td>Horn from the Heart: The Paul Butterfield Story</td>
      <td>Review: Paul Butterfieldâ€™s Story Is Told in ...</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td></td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>2018-10-19</td>
      <td>2018-10-16</td>
      <td>A documentary explores the life of the blues m...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A.O. SCOTT</td>
      <td>0</td>
      <td>2018-10-16 16:08:03</td>
      <td>The Price of Everything</td>
      <td>Review: â€˜The Price of Everythingâ€™ Asks $56...</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td></td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>2018-10-19</td>
      <td>2018-10-16</td>
      <td>This documentary examines the global art marke...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>BEN KENIGSBERG</td>
      <td>0</td>
      <td>2018-10-16 11:04:03</td>
      <td>Impulso</td>
      <td>Review: â€˜Impulsoâ€™ Goes Backstage With a Fl...</td>
      <td>{'type': 'article', 'url': 'http://www.nytimes...</td>
      <td></td>
      <td>{'type': 'mediumThreeByTwo210', 'src': 'https:...</td>
      <td>None</td>
      <td>2018-10-16</td>
      <td>This documentary follows RocÃ­o Molina, a cutt...</td>
    </tr>
  </tbody>
</table>
</div>



## How many unique critics are there?


```python
print(df.byline.unique())
print(df.byline.nunique())
```

    ['A.O. SCOTT' 'BEN KENIGSBERG' 'GLENN KENNY' 'JEANNETTE CATSOULIS'
     'MANOHLA DARGIS' 'KEN JAWOROWSKI' 'TEO BUGBEE']
    7
    

## Create a new column for the review's url. Title the column 'review_url'


```python
df['url'] = df.link.apply(lambda x: x['url'])
```

## How many results are in the file?


```python
len(df)
```




    20



## Summary
Well done! Here you continued to gather practice extracting data from JSON files and transforming them into our standard tool of Pandas DataFrames.
