---
redirect_from:
  - "/lessons/lesson14-python-for-data-science-i-o"
interact_link: content/lessons/Lesson14_Python_For_Data_Science_I_O.ipynb
kernel_name: python3
title: 'Lesson 14 - I/O'
prev_page:
  url: /lessons/Lesson13_Python_For_Data_Science_Sorting
  title: 'Lesson 13 - Sorting'
next_page:
  url: /lessons/Lesson15_Python_For_Data_Science_CaseStudies
  title: 'Lesson 15 - CaseStudies'
comment: "***PROGRAMMATICALLY GENERATED, DO NOT EDIT. SEE ORIGINAL FILES IN /content***"
---

<a href="https://colab.research.google.com/github/paiml/python_for_datascience/blob/master/Lesson14_Python_For_Data_Science_I_O.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

# Lesson 14: I/O

## Pragmatic AI Labs



![alt text](https://paiml.com/images/logo_with_slogan_white_background.png)

This notebook was produced by [Pragmatic AI Labs](https://paiml.com/).  You can continue learning about these topics by:

*   Buying a copy of [Pragmatic AI: An Introduction to Cloud-Based Machine Learning](http://www.informit.com/store/pragmatic-ai-an-introduction-to-cloud-based-machine-9780134863917)
*   Reading an online copy of [Pragmatic AI:Pragmatic AI: An Introduction to Cloud-Based Machine Learning](https://www.safaribooksonline.com/library/view/pragmatic-ai-an/9780134863924/)
*  Watching video [Essential Machine Learning and AI with Python and Jupyter Notebook-Video-SafariOnline](https://www.safaribooksonline.com/videos/essential-machine-learning/9780135261118) on Safari Books Online.
* Watching video [AWS Certified Machine Learning-Speciality](https://learning.oreilly.com/videos/aws-certified-machine/9780135556597)
* Purchasing video [Essential Machine Learning and AI with Python and Jupyter Notebook- Purchase Video](http://www.informit.com/store/essential-machine-learning-and-ai-with-python-and-jupyter-9780135261095)
*   Viewing more content at [noahgift.com](https://noahgift.com/)


## 14.1 Reading and Writing Files and Serializing Data in Python


### Working with Files

#### Writing to a file with 'context'



{:.input_area}
```
with open("food.txt", "w") as workfile:
    workfile.write("whey protein\n")
    workfile.write("cliff bar")
!cat food.txt
```


{:.output .output_stream}
```
whey protein
cliff bar
```

#### Reading a file with 'context'



{:.input_area}
```
with open("food.txt", "r") as workfile:
    #print(workfile.readlines())
    print(workfile.read())

```


{:.output .output_stream}
```
whey protein
cliff bar

```

### Serialization Techniques


##### Ingest



{:.input_area}
```
import pandas as pd
```




{:.input_area}
```
df = pd.read_csv(
    "https://raw.githubusercontent.com/noahgift/food/master/data/features.en.openfoodfacts.org.products.csv")
df.drop(["Unnamed: 0", "exceeded", "g_sum", "energy_100g"], axis=1, inplace=True) #drop two rows we don't need
df = df.drop(df.index[[1,11877]]) #drop outlier
df.rename(index=str, columns={"reconstructed_energy": "energy_100g"}, inplace=True)
df.head()
```





<div markdown="0" class="output output_html">
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
      <th>fat_100g</th>
      <th>carbohydrates_100g</th>
      <th>sugars_100g</th>
      <th>proteins_100g</th>
      <th>salt_100g</th>
      <th>energy_100g</th>
      <th>product</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>28.57</td>
      <td>64.29</td>
      <td>14.29</td>
      <td>3.57</td>
      <td>0.00000</td>
      <td>2267.85</td>
      <td>Banana Chips Sweetened (Whole)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>57.14</td>
      <td>17.86</td>
      <td>3.57</td>
      <td>17.86</td>
      <td>1.22428</td>
      <td>2835.70</td>
      <td>Organic Salted Nut Mix</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18.75</td>
      <td>57.81</td>
      <td>15.62</td>
      <td>14.06</td>
      <td>0.13970</td>
      <td>1953.04</td>
      <td>Organic Muesli</td>
    </tr>
    <tr>
      <th>4</th>
      <td>36.67</td>
      <td>36.67</td>
      <td>3.33</td>
      <td>16.67</td>
      <td>1.60782</td>
      <td>2336.91</td>
      <td>Zen Party Mix</td>
    </tr>
    <tr>
      <th>5</th>
      <td>18.18</td>
      <td>60.00</td>
      <td>21.82</td>
      <td>14.55</td>
      <td>0.02286</td>
      <td>1976.37</td>
      <td>Cinnamon Nut Granola</td>
    </tr>
  </tbody>
</table>
</div>
</div>



#### Serialize a Python Dictionary to Pickle



{:.input_area}
```
temp = df.head(1).to_dict('index')
mydict = list(temp.values())[0]
mydict
```





{:.output .output_data_text}
```
{'carbohydrates_100g': 64.29,
 'energy_100g': 2267.85,
 'fat_100g': 28.57,
 'product': 'Banana Chips Sweetened (Whole)',
 'proteins_100g': 3.57,
 'salt_100g': 0.0,
 'sugars_100g': 14.29}
```





{:.input_area}
```
import pickle
```




{:.input_area}
```
pickle.dump(mydict, open('mydictionary.pickle', 'wb'))
```




{:.input_area}
```
!ls -l mydictionary.pickle
```


{:.output .output_stream}
```
-rw-r--r-- 1 root root 225 Feb 14 19:30 mydictionary.pickle

```



{:.input_area}
```
res = pickle.load(open('mydictionary.pickle', "rb"))
```




{:.input_area}
```
res
```





{:.output .output_data_text}
```
{'carbohydrates_100g': 64.29,
 'energy_100g': 2267.85,
 'fat_100g': 28.57,
 'product': 'Banana Chips Sweetened (Whole)',
 'proteins_100g': 3.57,
 'salt_100g': 0.0,
 'sugars_100g': 14.29}
```



#### Serialize a Python Dictionary to JSON




{:.input_area}
```
import json
with open('data.json', 'w') as outfile:
    json.dump(res, outfile)
```




{:.input_area}
```
!cat data.json
```


{:.output .output_stream}
```
{"fat_100g": 28.57, "carbohydrates_100g": 64.29, "sugars_100g": 14.29, "proteins_100g": 3.57, "salt_100g": 0.0, "energy_100g": 2267.85, "product": "Banana Chips Sweetened (Whole)"}
```



{:.input_area}
```
with open('data.json', 'rb') as outfile:
    res2 = json.load(outfile)
```




{:.input_area}
```
res2
```





{:.output .output_data_text}
```
{'carbohydrates_100g': 64.29,
 'energy_100g': 2267.85,
 'fat_100g': 28.57,
 'product': 'Banana Chips Sweetened (Whole)',
 'proteins_100g': 3.57,
 'salt_100g': 0.0,
 'sugars_100g': 14.29}
```



#### Save to Yaml



{:.input_area}
```
import yaml
```




{:.input_area}
```
with open("data.yaml", "w") as yamlfile:                                               
    yaml.safe_dump(res2, yamlfile, default_flow_style=False)
```




{:.input_area}
```
!cat data.yaml
```


{:.output .output_stream}
```
carbohydrates_100g: 64.29
energy_100g: 2267.85
fat_100g: 28.57
product: Banana Chips Sweetened (Whole)
proteins_100g: 3.57
salt_100g: 0.0
sugars_100g: 14.29

```

#### Load Yaml



{:.input_area}
```
with open("data.yaml", "rb") as yamlfile:                                               
    res3 = yaml.safe_load(yamlfile) 
```




{:.input_area}
```
type(res3)
```





{:.output .output_data_text}
```
dict
```





{:.input_area}
```
res3
```





{:.output .output_data_text}
```
{'carbohydrates_100g': 64.29,
 'energy_100g': 2267.85,
 'fat_100g': 28.57,
 'product': 'Banana Chips Sweetened (Whole)',
 'proteins_100g': 3.57,
 'salt_100g': 0.0,
 'sugars_100g': 14.29}
```



## 14.2 Reading and Writing Files and Serializing Data with Pandas

### Use Pandas DataFrames


#### Creating Pandas DataFrames

##### Creating DataFrames CSV file



*   Can be local
*   Can be hosted on a website





{:.input_area}
```
df = pd.read_csv(
    "https://raw.githubusercontent.com/noahgift/food/master/data/features.en.openfoodfacts.org.products.csv")
df.drop(["Unnamed: 0", "exceeded", "g_sum", "energy_100g"], axis=1, inplace=True) #drop two rows we don't need
df = df.drop(df.index[[1,11877]]) #drop outlier
df.rename(index=str, columns={"reconstructed_energy": "energy_100g"}, inplace=True)
df.head()
```





<div markdown="0" class="output output_html">
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
      <th>fat_100g</th>
      <th>carbohydrates_100g</th>
      <th>sugars_100g</th>
      <th>proteins_100g</th>
      <th>salt_100g</th>
      <th>energy_100g</th>
      <th>product</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>28.57</td>
      <td>64.29</td>
      <td>14.29</td>
      <td>3.57</td>
      <td>0.00000</td>
      <td>2267.85</td>
      <td>Banana Chips Sweetened (Whole)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>57.14</td>
      <td>17.86</td>
      <td>3.57</td>
      <td>17.86</td>
      <td>1.22428</td>
      <td>2835.70</td>
      <td>Organic Salted Nut Mix</td>
    </tr>
    <tr>
      <th>3</th>
      <td>18.75</td>
      <td>57.81</td>
      <td>15.62</td>
      <td>14.06</td>
      <td>0.13970</td>
      <td>1953.04</td>
      <td>Organic Muesli</td>
    </tr>
    <tr>
      <th>4</th>
      <td>36.67</td>
      <td>36.67</td>
      <td>3.33</td>
      <td>16.67</td>
      <td>1.60782</td>
      <td>2336.91</td>
      <td>Zen Party Mix</td>
    </tr>
    <tr>
      <th>5</th>
      <td>18.18</td>
      <td>60.00</td>
      <td>21.82</td>
      <td>14.55</td>
      <td>0.02286</td>
      <td>1976.37</td>
      <td>Cinnamon Nut Granola</td>
    </tr>
  </tbody>
</table>
</div>
</div>





{:.input_area}
```
df.median()
```





{:.output .output_data_text}
```
fat_100g                 3.170
carbohydrates_100g      22.390
sugars_100g              5.880
proteins_100g            4.000
salt_100g                0.635
energy_100g           1121.540
dtype: float64
```



##### List to Pandas DataFrame

Convert a column to a list to a Pandas DataFrame



{:.input_area}
```
products = df['product'].tolist()
products_df = pd.DataFrame(products)
products_df.columns = ["product"]
products_df.head(3)


```





<div markdown="0" class="output output_html">
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
      <th>product</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Banana Chips Sweetened (Whole)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Organic Salted Nut Mix</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Organic Muesli</td>
    </tr>
  </tbody>
</table>
</div>
</div>





{:.input_area}
```
products_df.describe()
```





<div markdown="0" class="output output_html">
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
      <th>product</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>44976</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>30720</td>
    </tr>
    <tr>
      <th>top</th>
      <td>Ice Cream</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>122</td>
    </tr>
  </tbody>
</table>
</div>
</div>



#### Exporting Pandas DataFrames

##### Pandas DataFrame to CSV

Write out DataFrame using to_csv




{:.input_area}
```
df.head().to_csv("small_food_records.csv")
!cat small_food_records.csv
```


{:.output .output_stream}
```
,fat_100g,carbohydrates_100g,sugars_100g,proteins_100g,salt_100g,energy_100g,product
0,28.57,64.29,14.29,3.57,0.0,2267.85,Banana Chips Sweetened (Whole)
2,57.14,17.86,3.57,17.86,1.22428,2835.7,Organic Salted Nut Mix
3,18.75,57.81,15.62,14.06,0.1397,1953.04,Organic Muesli
4,36.67,36.67,3.33,16.67,1.60782,2336.91,Zen Party Mix
5,18.18,60.0,21.82,14.55,0.02286,1976.37,Cinnamon Nut Granola

```

#### Using Pandas on Ray

More info on Pandas:  https://rise.cs.berkeley.edu/blog/pandas-on-ray/

*Note:  Pandas is small data...data science.  You may need to use Spark on Pandas on Ray


#### Using Pandas on Dask

Dask natively scales Python

https://dask.org/

#### Using Google Sheets with Pandas DataFrames

Reference:  [Official Google Colab Documentation on IO](https://colab.research.google.com/notebooks/io.ipynb)

**Install Google Spreadsheet Library**



{:.input_area}
```
!pip install --upgrade -q gspread
```


**Authenticate to API**



{:.input_area}
```
from google.colab import auth
auth.authenticate_user()

import gspread
from oauth2client.client import GoogleCredentials

gc = gspread.authorize(GoogleCredentials.get_application_default())
```


**Create a Spreadsheet and Put Items in It**

Note, could use existing spreadsheet



{:.input_area}
```
sh = gc.create('pragmaticai-test')
worksheet = gc.open('pragmaticai-test').sheet1
cell_list = worksheet.range('A1:A10')

for cell in cell_list:
  cell.value = products.pop()
worksheet.update_cells(cell_list)
```





{:.output .output_data_text}
```
{'spreadsheetId': '1JUh0In3pmh6J7K5KQtGU1_6S9s3sB3cCa01meRIrYmU',
 'updatedCells': 10,
 'updatedColumns': 1,
 'updatedRange': 'Sheet1!A1:A10',
 'updatedRows': 10}
```



**Convert Spreadsheet Data to Pandas DataFrame**



{:.input_area}
```
worksheet = gc.open('pragmaticai-test').sheet1
rows = worksheet.get_all_values()
import pandas as pd
df = pd.DataFrame.from_records(rows)
print(df.median())
df

#df.head()
```


{:.output .output_stream}
```
Series([], dtype: float64)

```




<div markdown="0" class="output output_html">
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>100% Juice Reconstituted Lemon Juice With Adde...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cranberry Grape, 100% Juice Blend</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Cranberry Apple Flavored Juice Blended With On...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>100% Juice Blend</td>
    </tr>
    <tr>
      <th>4</th>
      <td>100% Juice, Prune Juice</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Prune Juice From Concentrate With Added Pulp</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Prune Juice From Concentrate</td>
    </tr>
    <tr>
      <th>7</th>
      <td>100% Juice Grape Juice</td>
    </tr>
    <tr>
      <th>8</th>
      <td>White Grape Juice</td>
    </tr>
    <tr>
      <th>9</th>
      <td>100% Apple Juice From Concentrate</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Whey Protein</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## 14.3 Reading and Writing using web resources

### Using Python Requests



{:.input_area}
```
import requests
url = """https://wikimedia.org/api/rest_v1/metrics/pageviews/per-article/en.wikipedia/all-access/user/LeBron_James/daily/2015070100/2017070500"""
result = requests.get(url)
result.json()["items"][0]

```





{:.output .output_data_text}
```
{'access': 'all-access',
 'agent': 'user',
 'article': 'LeBron_James',
 'granularity': 'daily',
 'project': 'en.wikipedia',
 'timestamp': '2015070100',
 'views': 18390}
```



### Using Boto



{:.input_area}
```
import boto3
resource = boto3.resource("s3")
resource.meta.client.download_file('testntest', 'nba_2017_endorsement_full_stats.csv',
'/tmp/nba_2017_endorsement_full_stats.csv')
```


### Using Github Files

### Using Kaggle Files

Use kaggle by mounting Google Drive with credentials

## 14.4 Using Function based concurrency

### Multiprocessing

#### Mapping processes to Functions

Processes are forked and run truly parallel (unlike threads)



{:.input_area}
```
from multiprocessing import Pool
import datetime
import time
import random

def fight_club(x):
  
    sleep_time = random.randrange(0,3)
    time.sleep(sleep_time)
    timestamp = datetime.datetime.now()
    print(f"Calculating punch with attack strength {x} to the {x} power: @timestamp {timestamp} with sleep {sleep_time}")
    return x**x

if __name__ == '__main__':
    p = Pool(5)
    print(p.map(fight_club, [1, 2, 3]))
```


{:.output .output_stream}
```
Calculating punch with attack strength 1 to the 1 power: @timestamp 2019-02-14 19:50:11.489381 with sleep 1
Calculating punch with attack strength 3 to the 3 power: @timestamp 2019-02-14 19:50:11.489381 with sleep 1
Calculating punch with attack strength 2 to the 2 power: @timestamp 2019-02-14 19:50:12.490516 with sleep 2
[1, 4, 27]

```

#### Process Pool Joined on Queue (Threadlike behavior)

Mimicks Threading interface, but with actual multi-core functionality



{:.input_area}
```
from multiprocessing import Process, Queue

def f(q):
    q.put(["armbar", "kimura",  "Mata Leão"])

if __name__ == '__main__':
    q = Queue()
    p = Process(target=f, args=(q,))
    p.start()
    print(f"Grabbing some attacks: {q.get()}")    
    p.join()
```


{:.output .output_stream}
```
Grabbing some attacks: ['armbar', 'kimura', 'Mata Leão']

```

### Async IO

#### Async IO in Python Examples

More info here:  https://docs.python.org/3/library/asyncio.html

**Using Python3 Async**

```python
import asyncio

def send_async_firehose_events(count=100):
    """Async sends events to firehose"""

    start = time.time() 
    client = firehose_client()
    extra_msg = {"aws_service": "firehose"}
    loop = asyncio.get_event_loop()
    tasks = []
    LOG.info(f"sending aysnc events TOTAL {count}",extra=extra_msg)
    num = 0
    for _ in range(count):
        tasks.append(asyncio.ensure_future(put_record(gen_uuid_events(), client)))
        LOG.info(f"sending aysnc events: COUNT {num}/{count}")
        num +=1
    loop.run_until_complete(asyncio.wait(tasks))
    loop.close()
    end = time.time()  
    LOG.info("Total time: {}".format(end - start))
  ```

**Using trollius library with Python 2:  DEPRECATED**

```python
"""Generates an Async MetaData call.  Note, this isn't available in Boto3
In [56]: res = all_metadata_async()
In [57]: res
Out[57]: 
[('ami-manifest-path', <Response [200]>),
 ('instance-type', <Response [200]>),
 ('instance-id', <Response [200]>),
 ('iam', <Response [200]>),
 ('local-hostname', <Response [200]>),
 ('network', <Response [200]>),
 ('hostname', <Response [200]>),
 ('ami-id', <Response [200]>),
 ('instance-action', <Response [200]>),
 ('profile', <Response [200]>),
 ('reservation-id', <Response [200]>),
 ('security-groups', <Response [200]>),
 ('metrics', <Response [200]>),
 ('mac', <Response [200]>),
 ('public-ipv4', <Response [200]>),
 ('services', <Response [200]>),
 ('local-ipv4', <Response [200]>),
 ('placement', <Response [200]>),
 ('ami-launch-index', <Response [200]>),
 ('public-hostname', <Response [200]>),
 ('public-keys', <Response [200]>),
 ('block-device-mapping', <Response [200]>)]
"""

import requests
import trollius

def get_metadata_api_urls():
    """Retrieves the api endpoints for metadata"""

    full_urls = {}
    metadata_url = "http://169.254.169.254/latest/meta-data/"
    resp = requests.get(metadata_url)
    urls = resp.content.split()
    for url in urls:
        stripped_url = url.rstrip("/")
        full_urls[stripped_url]=(os.path.join(metadata_url, url))
    return full_urls

def _get(key_url):
    key,url = key_url
    return key, requests.get(url)

def _do_calls(urls):
    loop = trollius.get_event_loop()
    futures = []
    for url in urls:
        futures.append(loop.run_in_executor(None, _get, url))
    return futures

@trollius.coroutine
def call():
    results = []
    futures = _do_calls(get_metadata_api_urls().items())
    for future in futures:
        result = yield trollius.From(future)
        results.append(result)
    raise trollius.Return(results)

def all_metadata_async():
    """Retrieves all available metadata for an instance async"""

    loop = trollius.get_event_loop()
    res = loop.run_until_complete(call())
   ```


### Serverless or FaaS (Functions as a service)

#### AWS Lambda

#####  AWS Lambda and Chalice Example

Standalone Lambda with Chalice:  http://chalice.readthedocs.io/en/latest/

```python
@app.lambda_function()
def send_message(event, context):
    """Send a message to a channel"""

    slack_client = SlackClient(SLACK_TOKEN)
    res = slack_client.api_call(
      "chat.postMessage",
      channel="#general",
      text=event
    )
    return res
```


#### Fn Project

##### Fn Project

![Fn Project](https://camo.githubusercontent.com/aad13cfe0e267f38143fd8cc6816ab8adde37a56/687474703a2f2f666e70726f6a6563742e696f2f696d616765732f666e2d333030783132352e706e67)


*   [FN Project](https://fnproject.io/)
*   [FN Project Python Example](http://fnproject.io/tutorials/python/intro/)



```bash

fn init --runtime python --trigger http pythonfn

```



```python

import fdk
import json


def handler(ctx, data=None, loop=None):
    name = "World"
    if data and len(data) > 0:
        body = json.loads(data)
        name = body.get("name")
    return {"message": "Hello {0}".format(name)}



if __name__ == "__main__":
    fdk.handle(handler)
```







### Large Scale Concurrency Solutions

#### Larger Scale Concurrency



*   [AWS Step Functions with Lambda](https://aws.amazon.com/step-functions/)

![alt text](https://d1.awsstatic.com/product-marketing/Step%20Functions/OrderFullScreen.0e74c2f19d89a9325addb5bd746cd895b2e4c9c2.jpg)

*   [AWS Batch](https://aws.amazon.com/batch/)
![alt text](https://d1.awsstatic.com/Test%20Images/Kate%20Test%20Images/Dilithium_flowchart%20diagrams_v3_kw-02.322877d73eda8ed71a44db216a1d195550befac0.png)

*   [RabbitMQ Worker Farms-IBM Developerworks Article](https://www.ibm.com/developerworks/cloud/library/cl-optimizepythoncloud1/index.html)

![alt text](https://www.ibm.com/developerworks/cloud/library/cl-optimizepythoncloud2/figure1.gif)





### High Level Concurrency Overview for Machine Learning and HPC (High Performance Computing)


#### Diagram of Python Performance Problems


![63,000X Speedup for Matrix Multiply from Standard Python](https://user-images.githubusercontent.com/58792/45932870-37339000-bf38-11e8-8272-bf2addf56df1.png)

Source:  [Dave Patterson, UC Berkeley](https://www2.eecs.berkeley.edu/Faculty/Homepages/patterson.html)

#### Numba

[Numba](http://numba.pydata.org/)

*   open source JIT (Just in Time Compiler)
*   translates a subset of Python and Numpy code into fast machine code
*   Can approach speed of C
*   Can also parallize:  "true threads" and "GPU"





##### Install Numba



{:.input_area}
```
!pip3 install numba
```


{:.output .output_stream}
```
Requirement already satisfied: numba in /usr/local/lib/python3.6/dist-packages (0.40.1)
Requirement already satisfied: numpy in /usr/local/lib/python3.6/dist-packages (from numba) (1.14.6)
Requirement already satisfied: llvmlite>=0.25.0dev0 in /usr/local/lib/python3.6/dist-packages (from numba) (0.27.0)

```

##### Use Numba



{:.input_area}
```
from numba import (cuda, vectorize)
import numba
import pandas as pd
import numpy as np
```




{:.input_area}
```
def real_estate_df():
    """30 Years of Housing Prices"""

    df = pd.read_csv("https://raw.githubusercontent.com/noahgift/real_estate_ml/master/data/Zip_Zhvi_SingleFamilyResidence.csv")
    df.rename(columns={"RegionName":"ZipCode"}, inplace=True)
    df["ZipCode"]=df["ZipCode"].map(lambda x: "{:.0f}".format(x))
    df["RegionID"]=df["RegionID"].map(lambda x: "{:.0f}".format(x))
    return df

def numerical_real_estate_array(df):
    """Converts df to numpy numerical array"""

    columns_to_drop = ['RegionID', 'ZipCode', 'City', 'State', 'Metro', 'CountyName']
    df_numerical = df.dropna()
    df_numerical = df_numerical.drop(columns_to_drop, axis=1)
    return df_numerical.values

def real_estate_array():
    """Returns Real Estate Array"""

    df = real_estate_df()
    rea = numerical_real_estate_array(df)
    return np.float32(rea)
  
rea = real_estate_array()
```


##### Use Numba decorator



{:.input_area}
```
import numba
```




{:.input_area}
```
@numba.jit(nopython=True)
def expmean_jit(rea):
    """Perform multiple mean calculations"""

    val = rea.mean() ** 2
    return val
  
expmean_jit(rea)
```





{:.output .output_data_text}
```
44968886272.0
```



##### Multi-threaded numba

True multi-threaded code (Warning will use all cores on anymachine that runs it)



{:.input_area}
```
@numba.jit(parallel=True)
def add_sum_threaded(rea):
    """Use all the cores"""

    x,_ = rea.shape
    total = 0
    for _ in numba.prange(x):
        total += rea.sum()  
        print(total)
        
add_sum_threaded(rea)
```


{:.output .output_stream}
```
550019399680.0550019399680.0

1100038799360.01100038799360.0

1650058199040.0
1650058199040.0
2200077598720.0
2200077598720.0
2750096998400.0
2750096998400.0
3300116398080.0
3300116398080.0
3850135797760.0
3850135797760.0
4400155197440.0
4950174597120.0
4400155197440.0
5500193996800.0
4950174597120.0
6050213396480.0
5500193996800.0
6600232796160.0
6050213396480.0
7150252195840.0
6600232796160.0
7700271595520.0
7150252195840.0
8250290995200.0
7700271595520.0
8800310394880.0
8250290995200.0
9350329794560.0
8800310394880.0
9900349194240.0
9350329794560.0
10450368593920.0
9900349194240.0
11000387993600.0
10450368593920.011550407393280.0

12100426792960.0
11000387993600.0
12650446192640.0
11550407393280.0
13200465592320.0
12100426792960.0
13750484992000.012650446192640.0

14300504391680.013200465592320.0

13750484992000.0
14850523791360.0
14300504391680.0
15400543191040.0
14850523791360.0
15950562590720.0
15400543191040.0
16500581990400.0
15950562590720.0
17050601390080.0
16500581990400.0
17600620789760.0
17050601390080.0
18150640189440.0
17600620789760.0
18700659589120.0
18150640189440.0
19250678988800.0
18700659589120.0
19800698388480.0
19250678988800.0
20350717788160.0
19800698388480.020350717788160.0

20900737187840.0
20900737187840.0
21450756587520.0
22000775987200.0
21450756587520.0
22550795386880.022000775987200.0

22550795386880.0
23100814786560.0
23100814786560.0
23650834186240.0
23650834186240.0
24200853585920.0
24200853585920.0
24750872985600.0
25300892385280.0
24750872985600.0
25850911784960.0
25300892385280.0
26400931184640.0
25850911784960.0
26950950584320.0
26400931184640.0
27500969984000.0
26950950584320.0
28050989383680.0
27500969984000.0
28601008783360.0
28050989383680.0
29151028183040.0
28601008783360.0
29701047582720.0
29151028183040.0
30251066982400.0
29701047582720.0
30801086382080.0
30251066982400.0
31351105781760.0
30801086382080.0
31901125181440.0
31351105781760.0
32451144581120.0
31901125181440.0
33001163980800.0
32451144581120.0
33551183380480.0
33001163980800.0
34101202780160.0
33551183380480.0
34651222179840.0
34101202780160.0
35201241579520.0
34651222179840.0
35751260979200.0
35201241579520.0
36301280378880.0
35751260979200.0
36851299778560.0
36301280378880.0
37401319178240.0
36851299778560.0
37951338577920.0
37401319178240.038501357977600.0

37951338577920.0
39051377377280.0
38501357977600.0
39601396776960.0
39051377377280.0
39601396776960.0
40151416176640.0
40151416176640.0
40701435576320.040701435576320.0

41251454976000.0
41251454976000.0
41801474375680.0
41801474375680.0
42351493775360.0
42351493775360.0
42901513175040.0
42901513175040.0
43451532574720.0
43451532574720.0
44001551974400.0
44001551974400.0
44551571374080.0
44551571374080.0
45101590773760.0
45101590773760.0
45651610173440.0
45651610173440.0
46201629573120.046201629573120.0

46751648972800.0
46751648972800.0
47301668372480.0
47301668372480.0
47851687772160.0
47851687772160.0
48401707171840.0
48401707171840.0
48951726571520.0
48951726571520.0
49501745971200.0
49501745971200.0
50051765370880.0
50051765370880.0
50601784770560.0
50601784770560.0
51151804170240.0
51151804170240.0
51701823569920.0
52251842969600.0
51701823569920.0
52801862369280.0
52251842969600.0
53351881768960.0
52801862369280.0
53901901168640.0
53351881768960.0
54451920568320.0
53901901168640.0
55001939968000.0
54451920568320.0
55551959367680.0
55001939968000.0
56101978767360.0
55551959367680.0
56651998167040.0
56101978767360.0
56651998167040.0
57202017566720.0
57202017566720.0
57752036966400.0
57752036966400.0
58302056366080.0
58302056366080.0
58852075765760.0
58852075765760.0
59402095165440.0
59402095165440.0
59952114565120.0
59952114565120.0
60502133964800.0
60502133964800.0
61052153364480.0
61602172764160.0
61052153364480.0
61602172764160.0
62152192163840.0
62152192163840.0
62702211563520.0
62702211563520.0
63252230963200.0
63252230963200.0
63802250362880.0
63802250362880.0
64352269762560.0
64352269762560.0
64902289162240.0
64902289162240.0
65452308561920.0
65452308561920.0
66002327961600.0
66002327961600.0
66552347361280.0
66552347361280.0
67102366760960.0
67102366760960.0
67652386160640.0
67652386160640.0
68202405560320.0
68202405560320.0
68752424960000.0
68752424960000.0
69302444359680.069302444359680.0

69852463759360.0
69852463759360.0
70402483159040.0
70402483159040.0
70952502558720.0
70952502558720.0
71502521958400.071502521958400.0

72052541358080.0
72052541358080.0
72602560757760.0
72602560757760.0
73152580157440.0
73152580157440.0
73702599557120.0
73702599557120.0
74252618956800.0
74252618956800.0
74802638356480.0
74802638356480.0
75352657756160.0
75352657756160.0
75902677155840.0
75902677155840.0
76452696555520.0
76452696555520.0
77002715955200.0
77002715955200.0
77552735354880.0
77552735354880.0
78102754754560.0
78102754754560.0
78652774154240.0
78652774154240.0
79202793553920.0
79202793553920.0
79752812953600.0
79752812953600.0
80302832353280.0
80302832353280.0
80852851752960.0
80852851752960.0
81402871152640.0
81402871152640.0
81952890552320.0
81952890552320.0
82502909952000.0
82502909952000.0
83052929351680.0
83602948751360.0
83052929351680.0
84152968151040.0
83602948751360.0
84702987550720.0
84152968151040.0
85253006950400.0
84702987550720.0
85803026350080.0
85253006950400.0
86353045749760.0
85803026350080.0
86903065149440.0
86353045749760.087453084549120.0

88003103948800.0
86903065149440.0
88553123348480.0
87453084549120.0
89103142748160.0
88003103948800.0
89653162147840.0
88553123348480.0
90203181547520.0
89103142748160.0
90753200947200.0
89653162147840.0
91303220346880.0
90203181547520.0
91853239746560.0
90753200947200.0
92403259146240.0
91303220346880.0
92953278545920.0
91853239746560.0
93503297945600.0
92403259146240.0
94053317345280.0
92953278545920.0
94603336744960.0
95153356144640.0
93503297945600.0
95703375544320.0
94053317345280.0
96253394944000.0
94603336744960.0
96803414343680.0
95153356144640.0
97353433743360.0
95703375544320.0
97903453143040.0
96253394944000.0
98453472542720.0
96803414343680.0
99003491942400.0
97353433743360.0
99553511342080.0
97903453143040.0
100103530741760.0
100653550141440.0
98453472542720.0
101203569541120.0
99003491942400.0
101753588940800.0
99553511342080.0
102303608340480.0
100103530741760.0
102853627740160.0
100653550141440.0
103403647139840.0
101203569541120.0
103953666539520.0
101753588940800.0
104503685939200.0
102303608340480.0
105053705338880.0
102853627740160.0
105603724738560.0
103403647139840.0
106153744138240.0
103953666539520.0
106703763537920.0
104503685939200.0
107253782937600.0
105053705338880.0
107803802337280.0
105603724738560.0
108353821736960.0
106153744138240.0
108903841136640.0
106703763537920.0
109453860536320.0
107253782937600.0
110003879936000.0
107803802337280.0
110553899335680.0
108353821736960.0
111103918735360.0
108903841136640.0
111653938135040.0
109453860536320.0
112203957534720.0
110003879936000.0
112753976934400.0
110553899335680.0
113303996334080.0
111103918735360.0
113854015733760.0
114404035133440.0111653938135040.0

112203957534720.0
114954054533120.0
115504073932800.0112753976934400.0

116054093332480.0
113303996334080.0
116604112732160.0
113854015733760.0
117154132131840.0
114404035133440.0
117704151531520.0
114954054533120.0
118254170931200.0
115504073932800.0
118804190330880.0
116054093332480.0
119354209730560.0
116604112732160.0
119904229130240.0
117154132131840.0
120454248529920.0
117704151531520.0121004267929600.0

121554287329280.0
118254170931200.0
122104306728960.0
118804190330880.0
122654326128640.0
119354209730560.0
123204345528320.0
119904229130240.0
123754364928000.0
120454248529920.0
124304384327680.0
121004267929600.0
124854403727360.0
121554287329280.0
125404423127040.0
122104306728960.0
125954442526720.0
122654326128640.0
126504461926400.0
123204345528320.0
127054481326080.0
123754364928000.0
127604500725760.0
124304384327680.0
128154520125440.0
124854403727360.0
128704539525120.0
125404423127040.0
129254558924800.0
129804578324480.0
125954442526720.0
130354597724160.0
126504461926400.0
130904617123840.0
127054481326080.0
131454636523520.0
127604500725760.0
132004655923200.0
128154520125440.0
132554675322880.0
128704539525120.0
133104694722560.0
129254558924800.0
133654714122240.0
129804578324480.0
134204733521920.0
130354597724160.0134754752921600.0

135304772321280.0
130904617123840.0
135854791720960.0
131454636523520.0
136404811120640.0
132004655923200.0
136954830520320.0
132554675322880.0
137504849920000.0
133104694722560.0138054869319680.0

133654714122240.0
138604888719360.0
134204733521920.0
139154908119040.0
134754752921600.0
139704927518720.0
135304772321280.0
140254946918400.0
135854791720960.0
140804966318080.0
136404811120640.0
141354985717760.0
136954830520320.0
141905005117440.0
137504849920000.0
142455024517120.0
138054869319680.0
143005043916800.0
138604888719360.0
143555063316480.0
139154908119040.0
144105082716160.0
139704927518720.0
140254946918400.0
140804966318080.0
141354985717760.0
144655102115840.0
145205121515520.0141905005117440.0

145755140915200.0
142455024517120.0
146305160314880.0
146855179714560.0
143005043916800.0
147405199114240.0
143555063316480.0
147955218513920.0
144105082716160.0
148505237913600.0
144655102115840.0149055257313280.0

145205121515520.0
149605276712960.0
145755140915200.0
150155296112640.0
146305160314880.0
150705315512320.0
146855179714560.0
151255334912000.0
147405199114240.0
151805354311680.0
147955218513920.0
152355373711360.0
148505237913600.0
152905393111040.0
149055257313280.0
153455412510720.0
149605276712960.0
154005431910400.0
150155296112640.0
150705315512320.0
154555451310080.0
151255334912000.0
155105470709760.0
151805354311680.0
155655490109440.0
152355373711360.0
156205509509120.0
152905393111040.0
156755528908800.0
153455412510720.0
154005431910400.0
157305548308480.0
154555451310080.0
157855567708160.0
155105470709760.0
158405587107840.0
155655490109440.0
158955606507520.0
156205509509120.0
159505625907200.0
156755528908800.0
160055645306880.0
157305548308480.0
160605664706560.0
157855567708160.0
161155684106240.0
158405587107840.0
161705703505920.0
158955606507520.0
162255722905600.0
159505625907200.0
162805742305280.0
160055645306880.0
163355761704960.0
160605664706560.0
163905781104640.0
161155684106240.0
164455800504320.0
161705703505920.0
165005819904000.0
162255722905600.0
165555839303680.0
166105858703360.0
162805742305280.0
166655878103040.0
163355761704960.0
167205897502720.0
163905781104640.0
167755916902400.0
164455800504320.0
168305936302080.0
165005819904000.0168855955701760.0

165555839303680.0
169405975101440.0
166105858703360.0
169955994501120.0
166655878103040.0
170506013900800.0
167205897502720.0
171056033300480.0
167755916902400.0
171606052700160.0
168305936302080.0
172156072099840.0
168855955701760.0
172706091499520.0
169405975101440.0
173256110899200.0
169955994501120.0
173806130298880.0170506013900800.0
171056033300480.0

171606052700160.0
174356149698560.0
172156072099840.0
174906169098240.0
172706091499520.0
175456188497920.0
173256110899200.0
176006207897600.0
173806130298880.0
176556227297280.0174356149698560.0

177106246696960.0
174906169098240.0
177656266096640.0
175456188497920.0
178206285496320.0
176006207897600.0
178756304896000.0
176556227297280.0
179306324295680.0
177106246696960.0
179856343695360.0
177656266096640.0
180406363095040.0
178206285496320.0180956382494720.0

178756304896000.0
181506401894400.0
179306324295680.0
182056421294080.0
179856343695360.0
182606440693760.0
180406363095040.0
183156460093440.0
180956382494720.0
183706479493120.0
181506401894400.0
184256498892800.0
182056421294080.0
184806518292480.0
182606440693760.0
185356537692160.0
183156460093440.0
185906557091840.0
183706479493120.0
186456576491520.0
184256498892800.0
187006595891200.0
184806518292480.0
187556615290880.0
185356537692160.0
188106634690560.0
185906557091840.0
188656654090240.0
186456576491520.0
189206673489920.0
187006595891200.0
187556615290880.0
189756692889600.0
188106634690560.0
190306712289280.0
188656654090240.0
190856731688960.0
189206673489920.0
191406751088640.0
189756692889600.0
191956770488320.0
190306712289280.0
192506789888000.0
190856731688960.0
193056809287680.0
191406751088640.0
193606828687360.0
191956770488320.0
194156848087040.0
192506789888000.0
194706867486720.0
193056809287680.0
195256886886400.0
193606828687360.0
195806906286080.0
194156848087040.0
196356925685760.0
194706867486720.0
196906945085440.0
195256886886400.0
195806906286080.0
197456964485120.0
196356925685760.0
198006983884800.0
196906945085440.0
198557003284480.0
197456964485120.0
199107022684160.0
198006983884800.0
199657042083840.0
198557003284480.0
200207061483520.0
199107022684160.0
200757080883200.0
199657042083840.0
201307100282880.0
200207061483520.0
201857119682560.0
200757080883200.0
202407139082240.0
201307100282880.0
202957158481920.0
203507177881600.0201857119682560.0

204057197281280.0
204607216680960.0
205157236080640.0
202407139082240.0
202957158481920.0
205707255480320.0
203507177881600.0
206257274880000.0
204057197281280.0
206807294279680.0
204607216680960.0
207357313679360.0
205157236080640.0
207907333079040.0
205707255480320.0
208457352478720.0
206257274880000.0
209007371878400.0
206807294279680.0
207357313679360.0209557391278080.0

210107410677760.0
207907333079040.0
210657430077440.0
208457352478720.0
211207449477120.0
209007371878400.0
211757468876800.0
209557391278080.0
212307488276480.0
210107410677760.0
212857507676160.0
210657430077440.0
213407527075840.0
211207449477120.0
213957546475520.0
211757468876800.0
214507565875200.0
212307488276480.0215057585274880.0

215607604674560.0
212857507676160.0
216157624074240.0
213407527075840.0
216707643473920.0
213957546475520.0
217257662873600.0
214507565875200.0
217807682273280.0
215057585274880.0
218357701672960.0
215607604674560.0
218907721072640.0
219457740472320.0
216157624074240.0
220007759872000.0
216707643473920.0
220557779271680.0217257662873600.0

217807682273280.0
221107798671360.0
218357701672960.0
221657818071040.0
218907721072640.0
222207837470720.0
219457740472320.0
222757856870400.0
220007759872000.0
223307876270080.0
220557779271680.0
223857895669760.0
221107798671360.0
224407915069440.0
221657818071040.0
224957934469120.0
222207837470720.0
225507953868800.0
222757856870400.0
226057973268480.0
223307876270080.0
226607992668160.0
223857895669760.0
227158012067840.0
224407915069440.0
227708031467520.0
224957934469120.0
228258050867200.0
225507953868800.0
228808070266880.0
226057973268480.0
229358089666560.0
226607992668160.0
229908109066240.0
227158012067840.0
230458128465920.0227708031467520.0

228258050867200.0
231008147865600.0
228808070266880.0
231558167265280.0
229358089666560.0
232108186664960.0
229908109066240.0
232658206064640.0
230458128465920.0
233208225464320.0
231008147865600.0
233758244864000.0
231558167265280.0
232108186664960.0
234308264263680.0
234858283663360.0
232658206064640.0
235408303063040.0
233208225464320.0
235958322462720.0
233758244864000.0
236508341862400.0
234308264263680.0
237058361262080.0
234858283663360.0
237608380661760.0
235408303063040.0
238158400061440.0
235958322462720.0
238708419461120.0
236508341862400.0
239258438860800.0
237058361262080.0
239808458260480.0
237608380661760.0
238158400061440.0
240358477660160.0
238708419461120.0240908497059840.0

241458516459520.0239258438860800.0

239808458260480.0
242008535859200.0
240358477660160.0
242558555258880.0
240908497059840.0
243108574658560.0
241458516459520.0
243658594058240.0
242008535859200.0
244208613457920.0
242558555258880.0
244758632857600.0
243108574658560.0
245308652257280.0
243658594058240.0
245858671656960.0
244208613457920.0
246408691056640.0
244758632857600.0
246958710456320.0
245308652257280.0
247508729856000.0
245858671656960.0
248058749255680.0
246408691056640.0
248608768655360.0
246958710456320.0
249158788055040.0
247508729856000.0
249708807454720.0
250258826854400.0248058749255680.0

248608768655360.0
250808846254080.0
249158788055040.0
251358865653760.0
249708807454720.0
251908885053440.0
250258826854400.0
252458904453120.0
250808846254080.0
253008923852800.0
251358865653760.0
253558943252480.0
251908885053440.0
254108962652160.0
252458904453120.0
254658982051840.0
255209001451520.0253008923852800.0

253558943252480.0
255759020851200.0
254108962652160.0
256309040250880.0
254658982051840.0
256859059650560.0
255209001451520.0
257409079050240.0
255759020851200.0
257959098449920.0
256309040250880.0
258509117849600.0
256859059650560.0
259059137249280.0
257409079050240.0
259609156648960.0
257959098449920.0
260159176048640.0
258509117849600.0
260709195448320.0
259059137249280.0
261259214848000.0
259609156648960.0
261809234247680.0
260159176048640.0
262359253647360.0
260709195448320.0
262909273047040.0
261259214848000.0
263459292446720.0
261809234247680.0
264009311846400.0
262359253647360.0
264559331246080.0
262909273047040.0
265109350645760.0
263459292446720.0
265659370045440.0
264009311846400.0
266209389445120.0
264559331246080.0
266759408844800.0
265109350645760.0
267309428244480.0
265659370045440.0
267859447644160.0
266209389445120.0
268409467043840.0
266759408844800.0268959486443520.0

267309428244480.0
269509505843200.0
267859447644160.0
270059525242880.0
268409467043840.0
270609544642560.0
268959486443520.0
271159564042240.0
269509505843200.0
271709583441920.0
270059525242880.0
272259602841600.0
270609544642560.0
272809622241280.0
271159564042240.0
273359641640960.0
271709583441920.0
273909661040640.0
272259602841600.0
274459680440320.0
275009699840000.0
272809622241280.0
275559719239680.0
276109738639360.0
273359641640960.0
276659758039040.0
273909661040640.0
274459680440320.0277209777438720.0

275009699840000.0
277759796838400.0
275559719239680.0
278309816238080.0
276109738639360.0
278859835637760.0
276659758039040.0
279409855037440.0
277209777438720.0
279959874437120.0
277759796838400.0
280509893836800.0
278309816238080.0
281059913236480.0
278859835637760.0
281609932636160.0
279409855037440.0
282159952035840.0
279959874437120.0
282709971435520.0
280509893836800.0
283259990835200.0
281059913236480.0
283810010234880.0
284360029634560.0281609932636160.0

282159952035840.0
284910049034240.0
282709971435520.0
285460068433920.0
283259990835200.0
286010087833600.0
283810010234880.0
286560107233280.0
284360029634560.0
287110126632960.0
284910049034240.0
287660146032640.0
285460068433920.0
288210165432320.0
286010087833600.0
288760184832000.0
286560107233280.0
289310204231680.0
287110126632960.0
289860223631360.0
287660146032640.0
290410243031040.0
288210165432320.0
290960262430720.0
288760184832000.0
291510281830400.0
289310204231680.0
292060301230080.0
289860223631360.0
290410243031040.0
290960262430720.0
292610320629760.0
291510281830400.0
293160340029440.0
292060301230080.0
293710359429120.0
292610320629760.0
294260378828800.0
293160340029440.0
294810398228480.0
293710359429120.0
295360417628160.0
294260378828800.0
295910437027840.0
294810398228480.0
296460456427520.0
295360417628160.0
297010475827200.0
297560495226880.0
295910437027840.0
298110514626560.0
296460456427520.0
298660534026240.0
297010475827200.0
299210553425920.0
297560495226880.0
299760572825600.0
298110514626560.0
300310592225280.0
298660534026240.0
300860611624960.0
299210553425920.0
301410631024640.0
299760572825600.0
301960650424320.0
300310592225280.0
302510669824000.0
300860611624960.0
303060689223680.0
303610708623360.0301410631024640.0

301960650424320.0
304160728023040.0
302510669824000.0
304710747422720.0
303060689223680.0
305260766822400.0
305810786222080.0
303610708623360.0
306360805621760.0
304160728023040.0
306910825021440.0
304710747422720.0
307460844421120.0
305260766822400.0
308010863820800.0305810786222080.0

306360805621760.0
308560883220480.0
306910825021440.0
309110902620160.0
307460844421120.0
309660922019840.0
308010863820800.0
310210941419520.0
308560883220480.0
310760960819200.0
309110902620160.0
311310980218880.0
309660922019840.0
311860999618560.0
310210941419520.0
312411019018240.0
310760960819200.0
312961038417920.0
311310980218880.0
313511057817600.0311860999618560.0

312411019018240.0
314061077217280.0
312961038417920.0
314611096616960.0
313511057817600.0
315161116016640.0
314061077217280.0315711135416320.0

314611096616960.0
316261154816000.0
315161116016640.0
316811174215680.0
315711135416320.0
317361193615360.0
316261154816000.0
317911213015040.0
316811174215680.0
318461232414720.0
317361193615360.0
319011251814400.0
317911213015040.0
319561271214080.0
318461232414720.0
320111290613760.0
319011251814400.0
320661310013440.0
319561271214080.0
321211329413120.0
320111290613760.0
321761348812800.0
320661310013440.0
322311368212480.0
321211329413120.0
322861387612160.0
321761348812800.0
323411407011840.0
322311368212480.0
323961426411520.0
322861387612160.0
324511445811200.0
323411407011840.0
325061465210880.0
323961426411520.0
325611484610560.0
324511445811200.0
326161504010240.0
326711523409920.0
325061465210880.0
327261542809600.0
325611484610560.0
327811562209280.0
326161504010240.0
328361581608960.0
328911601008640.0
326711523409920.0
329461620408320.0
327261542809600.0
330011639808000.0
327811562209280.0
330561659207680.0
328361581608960.0
331111678607360.0
328911601008640.0
329461620408320.0
331661698007040.0
330011639808000.0
332211717406720.0
330561659207680.0
332761736806400.0
331111678607360.0
333311756206080.0
331661698007040.0
333861775605760.0
332211717406720.0
334411795005440.0
332761736806400.0
334961814405120.0
333311756206080.0
335511833804800.0
333861775605760.0
336061853204480.0
334411795005440.0
336611872604160.0
337161892003840.0334961814405120.0

337711911403520.0
335511833804800.0
338261930803200.0336061853204480.0

338811950202880.0336611872604160.0

337161892003840.0
339361969602560.0
337711911403520.0
339911989002240.0
338261930803200.0
340462008401920.0
338811950202880.0
341012027801600.0
339361969602560.0
341562047201280.0
339911989002240.0
342112066600960.0
340462008401920.0
342662086000640.0
341012027801600.0
343212105400320.0
343762124800000.0
341562047201280.0
344312144199680.0
342112066600960.0
344862163599360.0
342662086000640.0
345412182999040.0
343212105400320.0
345962202398720.0
343762124800000.0
346512221798400.0
344312144199680.0
344862163599360.0
345412182999040.0
347062241198080.0
345962202398720.0
347612260597760.0
346512221798400.0
348162279997440.0
347062241198080.0
348712299397120.0
347612260597760.0
349262318796800.0
348162279997440.0
349812338196480.0
348712299397120.0
350362357596160.0
349262318796800.0
350912376995840.0
349812338196480.0351462396395520.0

352012415795200.0
350362357596160.0
352562435194880.0
350912376995840.0
353112454594560.0
351462396395520.0
353662473994240.0
352012415795200.0
354212493393920.0
352562435194880.0
354762512793600.0
353112454594560.0
355312532193280.0
353662473994240.0
355862551592960.0
354212493393920.0
356412570992640.0
354762512793600.0
356962590392320.0
355312532193280.0
357512609792000.0
355862551592960.0
358062629191680.0
356412570992640.0
358612648591360.0
356962590392320.0
359162667991040.0
357512609792000.0
359712687390720.0
358062629191680.0
360262706790400.0
358612648591360.0
360812726190080.0
359162667991040.0
361362745589760.0
359712687390720.0
361912764989440.0
360262706790400.0
362462784389120.0
360812726190080.0
363012803788800.0
363562823188480.0
364112842588160.0
361362745589760.0
361912764989440.0
364662861987840.0
365212881387520.0
362462784389120.0
365762900787200.0
363012803788800.0
366312920186880.0
363562823188480.0
364112842588160.0
366862939586560.0
364662861987840.0
367412958986240.0
367962978385920.0
368512997785600.0
365212881387520.0
369063017185280.0
365762900787200.0
369613036584960.0
366312920186880.0
370163055984640.0
366862939586560.0
370713075384320.0
367412958986240.0
371263094784000.0
367962978385920.0
371813114183680.0
368512997785600.0
372363133583360.0
369063017185280.0
372913152983040.0
369613036584960.0
373463172382720.0
370163055984640.0
374013191782400.0
370713075384320.0
374563211182080.0
371263094784000.0
375113230581760.0
371813114183680.0
375663249981440.0
372363133583360.0
376213269381120.0
372913152983040.0
376763288780800.0
373463172382720.0
377313308180480.0374013191782400.0

374563211182080.0
377863327580160.0
375113230581760.0
378413346979840.0
375663249981440.0
378963366379520.0
376213269381120.0
379513385779200.0
376763288780800.0
380063405178880.0
377313308180480.0
380613424578560.0
377863327580160.0
381163443978240.0
378413346979840.0
381713463377920.0
378963366379520.0382263482777600.0

382813502177280.0
379513385779200.0
383363521576960.0
380063405178880.0
383913540976640.0
380613424578560.0
384463560376320.0
381163443978240.0
385013579776000.0
381713463377920.0
385563599175680.0
382263482777600.0
386113618575360.0
382813502177280.0
386663637975040.0
383363521576960.0
387213657374720.0
383913540976640.0
387763676774400.0
384463560376320.0
388313696174080.0
385013579776000.0
388863715573760.0
385563599175680.0
389413734973440.0
386113618575360.0
389963754373120.0
386663637975040.0
390513773772800.0
387213657374720.0
391063793172480.0
387763676774400.0
391613812572160.0
388313696174080.0
392163831971840.0
388863715573760.0
392713851371520.0
389413734973440.0
393263870771200.0
389963754373120.0
393813890170880.0
390513773772800.0
394363909570560.0
391063793172480.0
394913928970240.0
395463948369920.0391613812572160.0

392163831971840.0
396013967769600.0
392713851371520.0
396563987169280.0
393263870771200.0
397114006568960.0
393813890170880.0
397664025968640.0
394363909570560.0
398214045368320.0
394913928970240.0
398764064768000.0
395463948369920.0
399314084167680.0
396013967769600.0
399864103567360.0
396563987169280.0
400414122967040.0
397114006568960.0
400964142366720.0
397664025968640.0
401514161766400.0
402064181166080.0
398214045368320.0
402614200565760.0
398764064768000.0
403164219965440.0399314084167680.0

399864103567360.0
403714239365120.0
400414122967040.0
404264258764800.0
400964142366720.0
404814278164480.0
401514161766400.0
405364297564160.0
402064181166080.0
405914316963840.0
402614200565760.0
406464336363520.0
403164219965440.0
407014355763200.0
403714239365120.0
407564375162880.0
404264258764800.0
408114394562560.0
404814278164480.0
408664413962240.0
405364297564160.0
409214433361920.0
405914316963840.0
409764452761600.0
406464336363520.0
410314472161280.0
407014355763200.0
410864491560960.0
407564375162880.0
411414510960640.0
408114394562560.0
411964530360320.0
408664413962240.0
412514549760000.0
409214433361920.0
413064569159680.0
409764452761600.0
413614588559360.0
410314472161280.0
414164607959040.0
410864491560960.0
414714627358720.0
411414510960640.0
415264646758400.0
411964530360320.0
415814666158080.0
412514549760000.0
416364685557760.0
413064569159680.0
416914704957440.0
413614588559360.0
417464724357120.0
414164607959040.0
418014743756800.0
414714627358720.0
418564763156480.0
415264646758400.0
419114782556160.0
415814666158080.0
419664801955840.0
416364685557760.0
420214821355520.0
416914704957440.0
420764840755200.0
421314860154880.0
417464724357120.0
421864879554560.0
418014743756800.0
422414898954240.0
418564763156480.0
422964918353920.0
419114782556160.0
423514937753600.0
419664801955840.0
424064957153280.0
420214821355520.0
424614976552960.0
420764840755200.0
421314860154880.0
425164995952640.0
421864879554560.0
425715015352320.0
422414898954240.0
426265034752000.0
422964918353920.0
426815054151680.0
423514937753600.0
427365073551360.0
424064957153280.0
427915092951040.0
424614976552960.0
428465112350720.0
425164995952640.0
429015131750400.0
425715015352320.0
429565151150080.0
426265034752000.0
430115170549760.0
426815054151680.0
430665189949440.0
427365073551360.0
431215209349120.0
427915092951040.0
431765228748800.0
428465112350720.0
432315248148480.0
429015131750400.0
432865267548160.0
433415286947840.0429565151150080.0

433965306347520.0
430115170549760.0
434515325747200.0
430665189949440.0
435065345146880.0
431215209349120.0
435615364546560.0
431765228748800.0
436165383946240.0
432315248148480.0
436715403345920.0
432865267548160.0
433415286947840.0
437265422745600.0
433965306347520.0
437815442145280.0
434515325747200.0
438365461544960.0
435065345146880.0
438915480944640.0
435615364546560.0
439465500344320.0
436165383946240.0
440015519744000.0
436715403345920.0
440565539143680.0
437265422745600.0
441115558543360.0
437815442145280.0
441665577943040.0
438365461544960.0
442215597342720.0
438915480944640.0
442765616742400.0
439465500344320.0
443315636142080.0
440015519744000.0
443865655541760.0
440565539143680.0
444415674941440.0
444965694341120.0
441115558543360.0
445515713740800.0
441665577943040.0
446065733140480.0
442215597342720.0
446615752540160.0
442765616742400.0
447165771939840.0
443315636142080.0
447715791339520.0
443865655541760.0
448265810739200.0
444415674941440.0448815830138880.0

444965694341120.0
449365849538560.0
445515713740800.0
449915868938240.0
446065733140480.0
450465888337920.0
446615752540160.0
451015907737600.0
447165771939840.0
451565927137280.0
447715791339520.0
452115946536960.0
448265810739200.0
452665965936640.0
448815830138880.0
453215985336320.0
449365849538560.0
453766004736000.0
449915868938240.0
454316024135680.0
450465888337920.0
454866043535360.0
451015907737600.0
451565927137280.0
455416062935040.0
452115946536960.0
452665965936640.0
455966082334720.0
453215985336320.0
456516101734400.0
453766004736000.0
457066121134080.0
454316024135680.0
457616140533760.0
458166159933440.0
454866043535360.0
458716179333120.0
455416062935040.0
459266198732800.0
455966082334720.0
459816218132480.0
456516101734400.0
460366237532160.0
457066121134080.0
460916256931840.0
457616140533760.0
461466276331520.0
462016295731200.0458166159933440.0

458716179333120.0
462566315130880.0
459266198732800.0463116334530560.0

463666353930240.0
459816218132480.0
464216373329920.0
460366237532160.0
464766392729600.0
460916256931840.0
465316412129280.0
461466276331520.0
465866431528960.0
462016295731200.0
466416450928640.0
462566315130880.0
466966470328320.0
463116334530560.0
467516489728000.0
463666353930240.0
468066509127680.0
464216373329920.0
468616528527360.0
464766392729600.0
469166547927040.0
465316412129280.0
469716567326720.0
465866431528960.0
470266586726400.0
466416450928640.0
470816606126080.0
466966470328320.0
471366625525760.0
467516489728000.0
468066509127680.0
471916644925440.0
472466664325120.0
473016683724800.0
468616528527360.0
473566703124480.0469166547927040.0

469716567326720.0
474116722524160.0
474666741923840.0
470266586726400.0
475216761323520.0470816606126080.0

471366625525760.0
475766780723200.0
471916644925440.0
476316800122880.0
472466664325120.0
476866819522560.0
473016683724800.0
477416838922240.0
473566703124480.0
477966858321920.0
474116722524160.0
478516877721600.0
479066897121280.0
474666741923840.0
479616916520960.0
475216761323520.0
475766780723200.0
480166935920640.0
476316800122880.0
480716955320320.0
476866819522560.0
481266974720000.0
477416838922240.0
481816994119680.0
477966858321920.0
482367013519360.0
478516877721600.0
482917032919040.0
479066897121280.0
483467052318720.0
479616916520960.0
484017071718400.0
480166935920640.0
484567091118080.0
480716955320320.0
485117110517760.0
481266974720000.0
485667129917440.0
481816994119680.0
486217149317120.0
482367013519360.0
486767168716800.0
482917032919040.0
487317188116480.0
483467052318720.0
487867207516160.0
484017071718400.0
488417226915840.0
484567091118080.0
488967246315520.0
485117110517760.0
489517265715200.0
490067285114880.0
485667129917440.0
490617304514560.0
491167323914240.0
486217149317120.0
491717343313920.0
486767168716800.0
492267362713600.0
487317188116480.0
492817382113280.0
487867207516160.0
493367401512960.0488417226915840.0

488967246315520.0
493917420912640.0
489517265715200.0
494467440312320.0
490067285114880.0
495017459712000.0
490617304514560.0
495567479111680.0
491167323914240.0
496117498511360.0
491717343313920.0
492267362713600.0
496667517911040.0
492817382113280.0
497217537310720.0
493367401512960.0
497767556710400.0
493917420912640.0
498317576110080.0
494467440312320.0
498867595509760.0
495017459712000.0
499417614909440.0
495567479111680.0
499967634309120.0
496117498511360.0
500517653708800.0
496667517911040.0501067673108480.0

497217537310720.0
501617692508160.0
497767556710400.0
502167711907840.0
498317576110080.0
502717731307520.0
498867595509760.0
503267750707200.0
499417614909440.0
503817770106880.0
499967634309120.0
504367789506560.0
500517653708800.0
504917808906240.0
501067673108480.0
505467828305920.0
501617692508160.0
506017847705600.0
502167711907840.0
506567867105280.0
502717731307520.0
507117886504960.0
503267750707200.0
507667905904640.0
503817770106880.0
508217925304320.0
504367789506560.0
504917808906240.0
508767944704000.0
505467828305920.0
509317964103680.0
506017847705600.0
509867983503360.0
506567867105280.0
510418002903040.0
507117886504960.0
510968022302720.0
507667905904640.0
511518041702400.0
508217925304320.0
512068061102080.0
508767944704000.0
512618080501760.0
509317964103680.0
513168099901440.0
509867983503360.0
513718119301120.0
510418002903040.0
514268138700800.0
510968022302720.0
514818158100480.0
511518041702400.0
515368177500160.0
512068061102080.0
515918196899840.0
512618080501760.0
516468216299520.0
513168099901440.0
517018235699200.0
513718119301120.0
517568255098880.0
514268138700800.0
518118274498560.0
514818158100480.0
518668293898240.0
515368177500160.0
519218313297920.0
515918196899840.0
519768332697600.0
516468216299520.0
520318352097280.0
517018235699200.0
520868371496960.0
517568255098880.0
521418390896640.0
518118274498560.0
521968410296320.0
518668293898240.0
522518429696000.0
519218313297920.0
523068449095680.0
519768332697600.0523618468495360.0

520318352097280.0
524168487895040.0
520868371496960.0
524718507294720.0
521418390896640.0
525268526694400.0
521968410296320.0
525818546094080.0
522518429696000.0
526368565493760.0
523068449095680.0
526918584893440.0
523618468495360.0
527468604293120.0
524168487895040.0
528018623692800.0
524718507294720.0
528568643092480.0
525268526694400.0
529118662492160.0
525818546094080.0
529668681891840.0
526368565493760.0
530218701291520.0
526918584893440.0
530768720691200.0
527468604293120.0
531318740090880.0
528018623692800.0
531868759490560.0
528568643092480.0
532418778890240.0
529118662492160.0
532968798289920.0
529668681891840.0533518817689600.0

534068837089280.0530218701291520.0

534618856488960.0530768720691200.0

531318740090880.0
535168875888640.0
531868759490560.0
535718895288320.0
532418778890240.0
536268914688000.0
532968798289920.0
536818934087680.0
533518817689600.0
537368953487360.0
534068837089280.0
537918972887040.0
534618856488960.0
538468992286720.0
535168875888640.0
539019011686400.0
535718895288320.0
539569031086080.0
536268914688000.0
540119050485760.0
536818934087680.0
540669069885440.0
537368953487360.0
541219089285120.0537918972887040.0

541769108684800.0538468992286720.0

539019011686400.0
542319128084480.0
539569031086080.0
542869147484160.0
540119050485760.0
543419166883840.0
540669069885440.0
543969186283520.0
541219089285120.0
544519205683200.0
541769108684800.0
545069225082880.0
542319128084480.0
545619244482560.0
542869147484160.0
546169263882240.0
543419166883840.0
543969186283520.0
546719283281920.0
547269302681600.0
544519205683200.0
547819322081280.0
545069225082880.0
548369341480960.0
545619244482560.0
548919360880640.0
546169263882240.0
549469380280320.0
546719283281920.0
550019399680000.0
547269302681600.0
550569419079680.0
547819322081280.0
551119438479360.0
548369341480960.0551669457879040.0

552219477278720.0548919360880640.0

549469380280320.0
552769496678400.0
553319516078080.0
550019399680000.0
553869535477760.0
550569419079680.0
554419554877440.0
551119438479360.0
554969574277120.0
551669457879040.0
555519593676800.0
552219477278720.0
556069613076480.0552769496678400.0

553319516078080.0
556619632476160.0
553869535477760.0
557169651875840.0
554419554877440.0
557719671275520.0
554969574277120.0
558269690675200.0
555519593676800.0
558819710074880.0
556069613076480.0
559369729474560.0
556619632476160.0
559919748874240.0
557169651875840.0
560469768273920.0
557719671275520.0
561019787673600.0
558269690675200.0
561569807073280.0558819710074880.0

559369729474560.0
562119826472960.0
559919748874240.0
562669845872640.0
560469768273920.0
563219865272320.0
561019787673600.0
563769884672000.0
561569807073280.0
564319904071680.0
562119826472960.0
564869923471360.0
562669845872640.0
565419942871040.0
563219865272320.0
565969962270720.0
563769884672000.0
566519981670400.0
564319904071680.0
567070001070080.0
564869923471360.0
567620020469760.0
565419942871040.0
568170039869440.0
565969962270720.0
566519981670400.0
568720059269120.0
567070001070080.0
569270078668800.0
567620020469760.0
569820098068480.0
570370117468160.0
568170039869440.0
570920136867840.0
568720059269120.0
571470156267520.0
569270078668800.0
572020175667200.0569820098068480.0

570370117468160.0
572570195066880.0
570920136867840.0
573120214466560.0
571470156267520.0
573670233866240.0
572020175667200.0
574220253265920.0
572570195066880.0
574770272665600.0
573120214466560.0
575320292065280.0
573670233866240.0
575870311464960.0
574220253265920.0
576420330864640.0
574770272665600.0
576970350264320.0
577520369664000.0
575320292065280.0
578070389063680.0
575870311464960.0
578620408463360.0
576420330864640.0
579170427863040.0
576970350264320.0
579720447262720.0
577520369664000.0
580270466662400.0
578070389063680.0
580820486062080.0
578620408463360.0
581370505461760.0
581920524861440.0
579170427863040.0
582470544261120.0
579720447262720.0
583020563660800.0
580270466662400.0
583570583060480.0
580820486062080.0
584120602460160.0
581370505461760.0
584670621859840.0
581920524861440.0
585220641259520.0
582470544261120.0
585770660659200.0
583020563660800.0
586320680058880.0583570583060480.0

584120602460160.0
586870699458560.0
584670621859840.0
587420718858240.0
585220641259520.0
587970738257920.0
588520757657600.0
585770660659200.0
586320680058880.0
589070777057280.0
586870699458560.0
589620796456960.0
587420718858240.0
590170815856640.0
590720835256320.0
587970738257920.0
591270854656000.0
588520757657600.0
591820874055680.0
589070777057280.0
592370893455360.0
589620796456960.0
592920912855040.0
590170815856640.0
593470932254720.0
590720835256320.0
594020951654400.0
591270854656000.0
594570971054080.0
591820874055680.0
595120990453760.0
592370893455360.0
595671009853440.0
592920912855040.0
596221029253120.0
593470932254720.0
596771048652800.0
594020951654400.0
597321068052480.0
594570971054080.0
597871087452160.0
595120990453760.0
598421106851840.0
595671009853440.0
598971126251520.0
596221029253120.0
599521145651200.0
600071165050880.0
596771048652800.0
600621184450560.0
597321068052480.0
601171203850240.0
597871087452160.0
601721223249920.0
598421106851840.0
602271242649600.0
598971126251520.0
602821262049280.0
599521145651200.0
603371281448960.0
600071165050880.0
603921300848640.0
600621184450560.0
604471320248320.0
601171203850240.0
605021339648000.0
601721223249920.0
605571359047680.0
602271242649600.0
606121378447360.0
602821262049280.0
606671397847040.0
603371281448960.0
607221417246720.0
603921300848640.0
607771436646400.0
604471320248320.0
605021339648000.0
608321456046080.0
605571359047680.0
608871475445760.0
609421494845440.0
606121378447360.0
609971514245120.0
606671397847040.0
610521533644800.0
607221417246720.0
611071553044480.0
607771436646400.0
611621572444160.0
608321456046080.0
612171591843840.0
608871475445760.0
612721611243520.0
609421494845440.0
613271630643200.0
609971514245120.0
613821650042880.0
610521533644800.0
614371669442560.0
611071553044480.0
614921688842240.0
611621572444160.0
615471708241920.0
612171591843840.0
616021727641600.0
612721611243520.0
616571747041280.0
613271630643200.0
617121766440960.0
613821650042880.0
617671785840640.0
618221805240320.0
614371669442560.0
618771824640000.0
614921688842240.0
619321844039680.0
615471708241920.0
619871863439360.0
616021727641600.0
620421882839040.0
616571747041280.0
620971902238720.0
621521921638400.0617121766440960.0

617671785840640.0
622071941038080.0
618221805240320.0
622621960437760.0
618771824640000.0623171979837440.0

623721999237120.0
619321844039680.0
624272018636800.0
619871863439360.0
624822038036480.0
620421882839040.0
625372057436160.0
620971902238720.0
625922076835840.0
626472096235520.0621521921638400.0

622071941038080.0
627022115635200.0
622621960437760.0
627572135034880.0
623171979837440.0
628122154434560.0
623721999237120.0
628672173834240.0
624272018636800.0
629222193233920.0
624822038036480.0
629772212633600.0
625372057436160.0
630322232033280.0
625922076835840.0
630872251432960.0
626472096235520.0
631422270832640.0
627022115635200.0
631972290232320.0
627572135034880.0
632522309632000.0
628122154434560.0
633072329031680.0
628672173834240.0
633622348431360.0
629222193233920.0
634172367831040.0
629772212633600.0634722387230720.0

630322232033280.0
635272406630400.0
630872251432960.0
635822426030080.0
636372445429760.0
631422270832640.0
636922464829440.0
631972290232320.0
637472484229120.0
632522309632000.0638022503628800.0

633072329031680.0
638572523028480.0
633622348431360.0
639122542428160.0
634172367831040.0
639672561827840.0
634722387230720.0
640222581227520.0
635272406630400.0
640772600627200.0
635822426030080.0
641322620026880.0
636372445429760.0
641872639426560.0
636922464829440.0
642422658826240.0
637472484229120.0
642972678225920.0
638022503628800.0
643522697625600.0
638572523028480.0
644072717025280.0
639122542428160.0
644622736424960.0
639672561827840.0
645172755824640.0
640222581227520.0
645722775224320.0
640772600627200.0
646272794624000.0
641322620026880.0
646822814023680.0
641872639426560.0
647372833423360.0
642422658826240.0
647922852823040.0
642972678225920.0
648472872222720.0
643522697625600.0
649022891622400.0
644072717025280.0
649572911022080.0
644622736424960.0
650122930421760.0
645172755824640.0
650672949821440.0
645722775224320.0
651222969221120.0
646272794624000.0
651772988620800.0
646822814023680.0
652323008020480.0
647372833423360.0
647922852823040.0
652873027420160.0
648472872222720.0
653423046819840.0
649022891622400.0
653973066219520.0
654523085619200.0
655073105018880.0
649572911022080.0
655623124418560.0
650122930421760.0
656173143818240.0
650672949821440.0
656723163217920.0
651222969221120.0
657273182617600.0
651772988620800.0
652323008020480.0
657823202017280.0
652873027420160.0
658373221416960.0
653423046819840.0
658923240816640.0
653973066219520.0
659473260216320.0
654523085619200.0
660023279616000.0
655073105018880.0
660573299015680.0
655623124418560.0
661123318415360.0
656173143818240.0
661673337815040.0
656723163217920.0
662223357214720.0
657273182617600.0
662773376614400.0
657823202017280.0
663323396014080.0
663873415413760.0
664423434813440.0
664973454213120.0
658373221416960.0665523473612800.0

666073493012480.0
658923240816640.0
666623512412160.0
659473260216320.0
667173531811840.0
660023279616000.0
667723551211520.0
660573299015680.0
668273570611200.0
661123318415360.0
668823590010880.0
661673337815040.0
669373609410560.0662223357214720.0

669923628810240.0
662773376614400.0
670473648209920.0
663323396014080.0
671023667609600.0
663873415413760.0
671573687009280.0
664423434813440.0
672123706408960.0
664973454213120.0
672673725808640.0
665523473612800.0
673223745208320.0
666073493012480.0
673773764608000.0
666623512412160.0
674323784007680.0
674873803407360.0667173531811840.0

675423822807040.0667723551211520.0

675973842206720.0
668273570611200.0
676523861606400.0
668823590010880.0
677073881006080.0
669373609410560.0
677623900405760.0
669923628810240.0
678173919805440.0
670473648209920.0
678723939205120.0
671023667609600.0
679273958604800.0
671573687009280.0
679823978004480.0
672123706408960.0
680373997404160.0
672673725808640.0
680924016803840.0
673223745208320.0
681474036203520.0
673773764608000.0
682024055603200.0
674323784007680.0
682574075002880.0
674873803407360.0
683124094402560.0
675423822807040.0
683674113802240.0675973842206720.0

676523861606400.0
684224133201920.0
677073881006080.0
684774152601600.0
677623900405760.0
685324172001280.0
678173919805440.0
685874191400960.0
678723939205120.0
686424210800640.0
679273958604800.0
686974230200320.0
679823978004480.0
687524249600000.0
680373997404160.0
688074268999680.0
680924016803840.0
688624288399360.0
681474036203520.0
689174307799040.0
689724327198720.0
682024055603200.0
690274346598400.0682574075002880.0

683124094402560.0
690824365998080.0
691374385397760.0683674113802240.0

684224133201920.0
691924404797440.0
684774152601600.0
692474424197120.0
685324172001280.0
693024443596800.0
685874191400960.0
693574462996480.0
686424210800640.0
694124482396160.0
686974230200320.0
694674501795840.0
687524249600000.0
695224521195520.0
688074268999680.0
695774540595200.0
688624288399360.0
696324559994880.0
689174307799040.0
696874579394560.0
689724327198720.0
697424598794240.0
690274346598400.0
697974618193920.0
690824365998080.0
698524637593600.0
691374385397760.0
699074656993280.0
691924404797440.0
699624676392960.0
692474424197120.0
700174695792640.0
693024443596800.0
700724715192320.0
693574462996480.0
701274734592000.0
694124482396160.0
701824753991680.0
694674501795840.0
702374773391360.0
695224521195520.0
702924792791040.0
695774540595200.0
703474812190720.0
696324559994880.0
704024831590400.0696874579394560.0

697424598794240.0
704574850990080.0
697974618193920.0
705124870389760.0
698524637593600.0
705674889789440.0
699074656993280.0
706224909189120.0
699624676392960.0
706774928588800.0
707324947988480.0
700174695792640.0
707874967388160.0
700724715192320.0
708424986787840.0
701274734592000.0
708975006187520.0
701824753991680.0
709525025587200.0
702374773391360.0
710075044986880.0
702924792791040.0
710625064386560.0
703474812190720.0
711175083786240.0
704024831590400.0
711725103185920.0
704574850990080.0
712275122585600.0
705124870389760.0
712825141985280.0
705674889789440.0
713375161384960.0
706224909189120.0
713925180784640.0
706774928588800.0
714475200184320.0
707324947988480.0
715025219584000.0
715575238983680.0707874967388160.0

708424986787840.0
716125258383360.0
708975006187520.0
716675277783040.0
709525025587200.0
717225297182720.0
710075044986880.0
717775316582400.0
710625064386560.0
718325335982080.0
711175083786240.0
718875355381760.0
711725103185920.0
719425374781440.0
712275122585600.0
719975394181120.0
712825141985280.0
720525413580800.0
713375161384960.0
721075432980480.0713925180784640.0

721625452380160.0
714475200184320.0
722175471779840.0
715025219584000.0
722725491179520.0
715575238983680.0
723275510579200.0
716125258383360.0
723825529978880.0
716675277783040.0
717225297182720.0
724375549378560.0
717775316582400.0724925568778240.0

725475588177920.0
718325335982080.0
726025607577600.0
718875355381760.0
726575626977280.0
719425374781440.0
727125646376960.0
719975394181120.0
727675665776640.0
720525413580800.0
728225685176320.0
721075432980480.0
728775704576000.0721625452380160.0

722175471779840.0
729325723975680.0
722725491179520.0
723275510579200.0
729875743375360.0
723825529978880.0
730425762775040.0
724375549378560.0
730975782174720.0
724925568778240.0
731525801574400.0
725475588177920.0
732075820974080.0
726025607577600.0
732625840373760.0
726575626977280.0
733175859773440.0
727125646376960.0
733725879173120.0
727675665776640.0
734275898572800.0
728225685176320.0
734825917972480.0
728775704576000.0
735375937372160.0
729325723975680.0
735925956771840.0
729875743375360.0
730425762775040.0
736475976171520.0
730975782174720.0
737025995571200.0
731525801574400.0
737576014970880.0
732075820974080.0
738126034370560.0
732625840373760.0
738676053770240.0
733175859773440.0
739226073169920.0
733725879173120.0
739776092569600.0
734275898572800.0
740326111969280.0
734825917972480.0
735375937372160.0
740876131368960.0
735925956771840.0
741426150768640.0
736475976171520.0
741976170168320.0737025995571200.0

737576014970880.0742526189568000.0

738126034370560.0
743076208967680.0
738676053770240.0
743626228367360.0
739226073169920.0
744176247767040.0
739776092569600.0
744726267166720.0
740326111969280.0
745276286566400.0
740876131368960.0
745826305966080.0
741426150768640.0
746376325365760.0
741976170168320.0
746926344765440.0
742526189568000.0
747476364165120.0
743076208967680.0
748026383564800.0
743626228367360.0
748576402964480.0
744176247767040.0
749126422364160.0
744726267166720.0
749676441763840.0
745276286566400.0
750226461163520.0
745826305966080.0
750776480563200.0
746376325365760.0
751326499962880.0
746926344765440.0
751876519362560.0
747476364165120.0
752426538762240.0748026383564800.0

748576402964480.0
752976558161920.0
749126422364160.0
753526577561600.0
749676441763840.0
754076596961280.0
750226461163520.0
754626616360960.0
750776480563200.0
755176635760640.0
751326499962880.0
755726655160320.0
751876519362560.0
756276674560000.0
752426538762240.0
756826693959680.0
752976558161920.0
757376713359360.0
753526577561600.0
757926732759040.0
758476752158720.0
754076596961280.0
759026771558400.0
754626616360960.0
759576790958080.0
755176635760640.0
760126810357760.0
755726655160320.0
760676829757440.0
756276674560000.0
756826693959680.0
761226849157120.0
757376713359360.0
761776868556800.0
757926732759040.0
762326887956480.0
758476752158720.0
762876907356160.0
759026771558400.0
763426926755840.0
759576790958080.0
763976946155520.0
760126810357760.0
764526965555200.0
760676829757440.0
765076984954880.0
761226849157120.0
765627004354560.0
761776868556800.0
766177023754240.0
762326887956480.0
766727043153920.0
762876907356160.0
767277062553600.0
763426926755840.0
767827081953280.0
763976946155520.0
768377101352960.0
764526965555200.0
768927120752640.0
765076984954880.0
769477140152320.0
765627004354560.0
770027159552000.0
766177023754240.0
770577178951680.0766727043153920.0

767277062553600.0
771127198351360.0
767827081953280.0
771677217751040.0
768377101352960.0
772227237150720.0
768927120752640.0
772777256550400.0
769477140152320.0
773327275950080.0
770027159552000.0
773877295349760.0
770577178951680.0
774427314749440.0
771127198351360.0
774977334149120.0
771677217751040.0
775527353548800.0
772227237150720.0
776077372948480.0
772777256550400.0
776627392348160.0
773327275950080.0
777177411747840.0
773877295349760.0
777727431147520.0
774427314749440.0
778277450547200.0
774977334149120.0
778827469946880.0
779377489346560.0
775527353548800.0
779927508746240.0
776077372948480.0
780477528145920.0
776627392348160.0
781027547545600.0
777177411747840.0
781577566945280.0
777727431147520.0
782127586344960.0
778277450547200.0
782677605744640.0
778827469946880.0
783227625144320.0
779377489346560.0
783777644544000.0
784327663943680.0779927508746240.0

780477528145920.0
784877683343360.0
781027547545600.0
785427702743040.0
781577566945280.0
785977722142720.0
782127586344960.0
786527741542400.0
782677605744640.0
787077760942080.0
783227625144320.0
787627780341760.0
783777644544000.0
788177799741440.0
784327663943680.0
788727819141120.0
784877683343360.0
789277838540800.0
785427702743040.0
789827857940480.0785977722142720.0

790377877340160.0786527741542400.0

787077760942080.0
790927896739840.0
787627780341760.0
791477916139520.0
788177799741440.0
792027935539200.0
788727819141120.0
792577954938880.0
789277838540800.0
793127974338560.0
789827857940480.0
793677993738240.0
790377877340160.0
794228013137920.0
794778032537600.0
790927896739840.0
795328051937280.0791477916139520.0

792027935539200.0
795878071336960.0
792577954938880.0
796428090736640.0
793127974338560.0
796978110136320.0
793677993738240.0
797528129536000.0
794228013137920.0
798078148935680.0
794778032537600.0
798628168335360.0
795328051937280.0
799178187735040.0
795878071336960.0
799728207134720.0
796428090736640.0
800278226534400.0
796978110136320.0
800828245934080.0
797528129536000.0
801378265333760.0
801928284733440.0
798078148935680.0
802478304133120.0
798628168335360.0
803028323532800.0
799178187735040.0
803578342932480.0
799728207134720.0
804128362332160.0
800278226534400.0
804678381731840.0
800828245934080.0
805228401131520.0
801378265333760.0
805778420531200.0
801928284733440.0
806328439930880.0
802478304133120.0
806878459330560.0
803028323532800.0
807428478730240.0
803578342932480.0
807978498129920.0
804128362332160.0
804678381731840.0
808528517529600.0
805228401131520.0
809078536929280.0
805778420531200.0
809628556328960.0
806328439930880.0
810178575728640.0
806878459330560.0
810728595128320.0
807428478730240.0
811278614528000.0
807978498129920.0
811828633927680.0
808528517529600.0
809078536929280.0
812378653327360.0
809628556328960.0
812928672727040.0
810178575728640.0
813478692126720.0
810728595128320.0
814028711526400.0
811278614528000.0
814578730926080.0
811828633927680.0
815128750325760.0
812378653327360.0
815678769725440.0
812928672727040.0
816228789125120.0813478692126720.0

816778808524800.0
814028711526400.0
817328827924480.0
814578730926080.0
817878847324160.0
818428866723840.0
815128750325760.0
818978886123520.0
815678769725440.0
819528905523200.0
816228789125120.0
820078924922880.0
816778808524800.0
820628944322560.0
817328827924480.0
821178963722240.0
817878847324160.0
821728983121920.0818428866723840.0

822279002521600.0818978886123520.0

819528905523200.0
822829021921280.0
820078924922880.0
823379041320960.0
820628944322560.0
823929060720640.0
821178963722240.0
824479080120320.0
821728983121920.0
825029099520000.0
822279002521600.0
825579118919680.0
822829021921280.0
826129138319360.0
823379041320960.0
826679157719040.0
823929060720640.0
827229177118720.0
824479080120320.0
827779196518400.0
828329215918080.0
825029099520000.0
828879235317760.0
825579118919680.0
829429254717440.0
826129138319360.0
829979274117120.0
826679157719040.0
830529293516800.0
827229177118720.0
831079312916480.0
827779196518400.0
831629332316160.0
828329215918080.0
832179351715840.0
828879235317760.0
832729371115520.0
829429254717440.0
833279390515200.0
829979274117120.0
833829409914880.0
830529293516800.0
834379429314560.0
831079312916480.0
834929448714240.0
831629332316160.0
835479468113920.0
832179351715840.0
836029487513600.0
832729371115520.0
836579506913280.0
833279390515200.0
837129526312960.0
833829409914880.0
837679545712640.0
834379429314560.0
838229565112320.0
834929448714240.0
838779584512000.0
835479468113920.0
839329603911680.0
836029487513600.0
839879623311360.0
836579506913280.0
840429642711040.0
837129526312960.0
840979662110720.0
837679545712640.0
841529681510400.0
838229565112320.0
842079700910080.0
838779584512000.0
839329603911680.0
842629720309760.0
839879623311360.0
843179739709440.0
840429642711040.0
843729759109120.0
840979662110720.0
844279778508800.0
841529681510400.0
844829797908480.0
842079700910080.0
845379817308160.0
842629720309760.0
845929836707840.0
843179739709440.0
846479856107520.0
843729759109120.0
847029875507200.0
844279778508800.0
847579894906880.0
844829797908480.0
848129914306560.0
845379817308160.0
848679933706240.0
845929836707840.0
849229953105920.0
846479856107520.0
849779972505600.0
847029875507200.0
850329991905280.0
847579894906880.0
850880011304960.0
848129914306560.0
851430030704640.0
851980050104320.0848679933706240.0

852530069504000.0849229953105920.0

849779972505600.0853080088903680.0

853630108303360.0
850329991905280.0
854180127703040.0
850880011304960.0
854730147102720.0
851430030704640.0
855280166502400.0
851980050104320.0
855830185902080.0
852530069504000.0
856380205301760.0
853080088903680.0
856930224701440.0
853630108303360.0
857480244101120.0
854180127703040.0
858030263500800.0
854730147102720.0
858580282900480.0
855280166502400.0
859130302300160.0
855830185902080.0
859680321699840.0
856380205301760.0
860230341099520.0
856930224701440.0
860780360499200.0
857480244101120.0
861330379898880.0
858030263500800.0
861880399298560.0
858580282900480.0
862430418698240.0
859130302300160.0862980438097920.0

863530457497600.0
859680321699840.0
864080476897280.0
860230341099520.0
864630496296960.0
860780360499200.0
865180515696640.0
861330379898880.0
865730535096320.0
861880399298560.0
866280554496000.0
862430418698240.0
866830573895680.0
862980438097920.0
867380593295360.0
863530457497600.0
867930612695040.0864080476897280.0

868480632094720.0
864630496296960.0
869030651494400.0
865180515696640.0
869580670894080.0
865730535096320.0
870130690293760.0
866280554496000.0
870680709693440.0
866830573895680.0
871230729093120.0
867380593295360.0
871780748492800.0
867930612695040.0
872330767892480.0
868480632094720.0
872880787292160.0
873430806691840.0
869030651494400.0
873980826091520.0
869580670894080.0
870130690293760.0
874530845491200.0
870680709693440.0
875080864890880.0
871230729093120.0
875630884290560.0
871780748492800.0
876180903690240.0
872330767892480.0
876730923089920.0
872880787292160.0
877280942489600.0
873430806691840.0
877830961889280.0
873980826091520.0
878380981288960.0874530845491200.0

875080864890880.0
878931000688640.0
875630884290560.0
879481020088320.0
876180903690240.0
880031039488000.0
876730923089920.0
880581058887680.0
877280942489600.0
881131078287360.0
877830961889280.0
881681097687040.0
878380981288960.0
882231117086720.0
878931000688640.0
882781136486400.0
879481020088320.0
883331155886080.0
880031039488000.0
883881175285760.0
880581058887680.0
884431194685440.0
881131078287360.0
884981214085120.0
881681097687040.0
885531233484800.0
882231117086720.0
886081252884480.0
882781136486400.0
886631272284160.0
883331155886080.0
887181291683840.0
883881175285760.0
887731311083520.0
884431194685440.0
888281330483200.0
884981214085120.0
888831349882880.0
885531233484800.0
889381369282560.0
886081252884480.0
889931388682240.0
886631272284160.0
890481408081920.0
887181291683840.0
891031427481600.0
887731311083520.0
891581446881280.0
888281330483200.0
892131466280960.0
888831349882880.0
892681485680640.0
889381369282560.0
893231505080320.0
889931388682240.0
893781524480000.0
890481408081920.0
894331543879680.0
891031427481600.0
894881563279360.0
891581446881280.0
895431582679040.0
892131466280960.0
895981602078720.0
892681485680640.0
896531621478400.0
893231505080320.0
897081640878080.0
893781524480000.0
897631660277760.0894331543879680.0

894881563279360.0
898181679677440.0
895431582679040.0
898731699077120.0
895981602078720.0
899281718476800.0
896531621478400.0
899831737876480.0
897081640878080.0
900381757276160.0
897631660277760.0
900931776675840.0898181679677440.0

898731699077120.0
901481796075520.0
899281718476800.0
902031815475200.0
899831737876480.0
902581834874880.0
900381757276160.0
900931776675840.0
903131854274560.0
901481796075520.0
903681873674240.0
902031815475200.0
904231893073920.0
902581834874880.0
904781912473600.0
903131854274560.0
905331931873280.0
903681873674240.0
905881951272960.0
904231893073920.0
906431970672640.0
906981990072320.0904781912473600.0

907532009472000.0905331931873280.0

905881951272960.0
908082028871680.0
906431970672640.0
908632048271360.0
906981990072320.0
909182067671040.0
907532009472000.0
909732087070720.0
908082028871680.0
910282106470400.0
908632048271360.0
909182067671040.0
910832125870080.0
909732087070720.0
911382145269760.0
910282106470400.0
911932164669440.0
910832125870080.0
912482184069120.0
911382145269760.0
913032203468800.0
911932164669440.0
913582222868480.0
912482184069120.0
914132242268160.0
913032203468800.0
914682261667840.0
913582222868480.0
915232281067520.0
914132242268160.0
914682261667840.0
915782300467200.0
915232281067520.0
916332319866880.0
915782300467200.0
916882339266560.0
916332319866880.0
917432358666240.0
916882339266560.0
917982378065920.0
917432358666240.0
918532397465600.0
917982378065920.0
919082416865280.0
919632436264960.0
918532397465600.0
919082416865280.0
920182455664640.0
919632436264960.0
920732475064320.0
920182455664640.0
921282494464000.0
921832513863680.0920732475064320.0

921282494464000.0
922382533263360.0
921832513863680.0
922932552663040.0
922382533263360.0
923482572062720.0
924032591462400.0
922932552663040.0
924582610862080.0
923482572062720.0
925132630261760.0
924032591462400.0
925682649661440.0
924582610862080.0
926232669061120.0
925132630261760.0
926782688460800.0
925682649661440.0
927332707860480.0
926232669061120.0
927882727260160.0
926782688460800.0
928432746659840.0
927332707860480.0
927882727260160.0
928982766059520.0
928432746659840.0
929532785459200.0
928982766059520.0
930082804858880.0
930632824258560.0
929532785459200.0
930082804858880.0
931182843658240.0
931732863057920.0930632824258560.0

931182843658240.0
932282882457600.0
931732863057920.0
932832901857280.0
932282882457600.0
933382921256960.0
932832901857280.0933932940656640.0

934482960056320.0
933382921256960.0
935032979456000.0
933932940656640.0
935582998855680.0
934482960056320.0
936133018255360.0
935032979456000.0
936683037655040.0
935582998855680.0937233057054720.0

937783076454400.0
936133018255360.0
938333095854080.0
936683037655040.0
938883115253760.0
937233057054720.0
939433134653440.0
937783076454400.0
939983154053120.0
938333095854080.0
940533173452800.0
938883115253760.0
941083192852480.0
939433134653440.0
941633212252160.0
942183231651840.0939983154053120.0

940533173452800.0
942733251051520.0
941083192852480.0
943283270451200.0
941633212252160.0
943833289850880.0
942183231651840.0
944383309250560.0942733251051520.0

943283270451200.0
944933328650240.0
943833289850880.0
945483348049920.0
944383309250560.0
946033367449600.0
944933328650240.0
946583386849280.0
945483348049920.0
947133406248960.0
946033367449600.0
947683425648640.0
946583386849280.0
948233445048320.0
947133406248960.0
948783464448000.0
947683425648640.0
949333483847680.0
948233445048320.0
949883503247360.0
948783464448000.0
950433522647040.0
949333483847680.0
950983542046720.0
949883503247360.0
951533561446400.0
950433522647040.0
952083580846080.0
950983542046720.0
952633600245760.0
951533561446400.0
953183619645440.0
952083580846080.0
953733639045120.0
952633600245760.0
954283658444800.0
953183619645440.0
954833677844480.0
953733639045120.0
955383697244160.0
954283658444800.0
955933716643840.0
954833677844480.0
956483736043520.0
955383697244160.0
957033755443200.0
955933716643840.0
957583774842880.0
956483736043520.0
958133794242560.0
957033755443200.0
958683813642240.0
957583774842880.0
959233833041920.0
958133794242560.0
959783852441600.0
958683813642240.0
960333871841280.0
959233833041920.0
960883891240960.0
959783852441600.0
961433910640640.0
960333871841280.0
961983930040320.0
962533949440000.0
960883891240960.0
963083968839680.0
961433910640640.0
963633988239360.0
961983930040320.0
964184007639040.0
962533949440000.0
964734027038720.0
963083968839680.0
965284046438400.0
963633988239360.0
965834065838080.0
964184007639040.0
966384085237760.0
964734027038720.0
966934104637440.0
965284046438400.0
967484124037120.0
965834065838080.0
968034143436800.0
966384085237760.0
968584162836480.0
966934104637440.0
969134182236160.0
967484124037120.0
969684201635840.0
968034143436800.0
970234221035520.0
968584162836480.0
970784240435200.0
969134182236160.0
971334259834880.0
969684201635840.0
971884279234560.0
970234221035520.0
972434298634240.0
970784240435200.0
972984318033920.0
971334259834880.0
973534337433600.0
971884279234560.0
974084356833280.0
972434298634240.0
974634376232960.0
972984318033920.0
975184395632640.0
973534337433600.0
975734415032320.0
974084356833280.0
976284434432000.0
974634376232960.0
976834453831680.0
975184395632640.0
977384473231360.0
975734415032320.0
977934492631040.0
976284434432000.0
978484512030720.0
976834453831680.0
979034531430400.0
977384473231360.0
979584550830080.0
977934492631040.0
980134570229760.0
980684589629440.0
978484512030720.0
981234609029120.0
979034531430400.0
981784628428800.0
979584550830080.0
982334647828480.0
980134570229760.0
982884667228160.0
980684589629440.0
983434686627840.0
981234609029120.0
983984706027520.0
981784628428800.0
984534725427200.0
982334647828480.0
985084744826880.0
982884667228160.0
985634764226560.0
983434686627840.0
986184783626240.0
983984706027520.0
986734803025920.0
984534725427200.0
987284822425600.0
985084744826880.0
987834841825280.0
985634764226560.0
988384861224960.0
986184783626240.0
988934880624640.0
986734803025920.0
989484900024320.0
987284822425600.0
990034919424000.0
990584938823680.0987834841825280.0

988384861224960.0
991134958223360.0
988934880624640.0
991684977623040.0
989484900024320.0
992234997022720.0
990034919424000.0
992785016422400.0
990584938823680.0
993335035822080.0
991134958223360.0
993885055221760.0
991684977623040.0
994435074621440.0
992234997022720.0
994985094021120.0
992785016422400.0
995535113420800.0
993335035822080.0
996085132820480.0993885055221760.0

994435074621440.0
996635152220160.0
994985094021120.0
997185171619840.0
995535113420800.0
997735191019520.0
996085132820480.0
998285210419200.0
996635152220160.0
998835229818880.0
997185171619840.0
999385249218560.0
997735191019520.0999935268618240.0

1000485288017920.0
998285210419200.0
998835229818880.01001035307417600.0

999385249218560.0
1001585326817280.0
999935268618240.01002135346216960.0

1000485288017920.0
1002685365616640.0
1001035307417600.0
1003235385016320.0
1001585326817280.0
1003785404416000.0
1002135346216960.01004335423815680.0

1004885443215360.0
1002685365616640.0
1005435462615040.0
1003235385016320.0
1005985482014720.0
1003785404416000.0
1006535501414400.0
1004335423815680.0
1007085520814080.0
1004885443215360.0
1007635540213760.0
1005435462615040.0
1008185559613440.0
1005985482014720.0
1008735579013120.0
1006535501414400.0
1009285598412800.0
1007085520814080.0
1009835617812480.0
1007635540213760.0
1010385637212160.0
1008185559613440.0
1010935656611840.0
1008735579013120.0
1011485676011520.0
1009285598412800.0
1012035695411200.0
1009835617812480.0
1012585714810880.0
1010385637212160.0
1013135734210560.0
1010935656611840.0
1013685753610240.0
1011485676011520.0
1014235773009920.0
1012035695411200.0
1014785792409600.0
1012585714810880.0
1015335811809280.0
1013135734210560.0
1013685753610240.0
1015885831208960.0
1014235773009920.0
1016435850608640.0
1014785792409600.0
1016985870008320.0
1015335811809280.0
1017535889408000.01015885831208960.0

1018085908807680.01016435850608640.0

1016985870008320.0
1018635928207360.0
1017535889408000.0
1019185947607040.0
1018085908807680.0
1019735967006720.0
1018635928207360.0
1020285986406400.0
1020836005806080.0
1019185947607040.0
1019735967006720.0
1021386025205760.0
1020285986406400.0
1021936044605440.0
1020836005806080.0
1022486064005120.0
1021386025205760.0
1023036083404800.0
1021936044605440.0
1023586102804480.0
1022486064005120.0
1024136122204160.0
1023036083404800.0
1024686141603840.0
1023586102804480.01025236161003520.0

1025786180403200.0
1024136122204160.0
1026336199802880.0
1024686141603840.0
1026886219202560.0
1025236161003520.0
1027436238602240.0
1025786180403200.0
1027986258001920.0
1026336199802880.0
1028536277401600.0
1026886219202560.0
1029086296801280.0
1029636316200960.0
1027436238602240.0
1030186335600640.0
1027986258001920.0
1030736355000320.01028536277401600.0

1029086296801280.0
1031286374400000.0
1029636316200960.0
1031836393799680.0
1030186335600640.0
1032386413199360.0
1030736355000320.0
1032936432599040.0
1031286374400000.0
1033486451998720.0
1031836393799680.0
1034036471398400.0
1032386413199360.0
1034586490798080.0
1035136510197760.0
1032936432599040.0
1035686529597440.0
1033486451998720.0
1036236548997120.0
1034036471398400.0
1036786568396800.0
1034586490798080.0
1037336587796480.0
1035136510197760.0
1037886607196160.0
1035686529597440.0
1038436626595840.0
1036236548997120.0
1038986645995520.0
1036786568396800.0
1039536665395200.0
1037336587796480.0
1040086684794880.0
1037886607196160.0
1040636704194560.0
1038436626595840.0
1041186723594240.0
1038986645995520.0
1041736742993920.0
1039536665395200.0
1042286762393600.0
1040086684794880.0
1042836781793280.0
1040636704194560.0
1043386801192960.0
1041186723594240.01043936820592640.0

1041736742993920.0
1044486839992320.0
1042286762393600.0
1045036859392000.0
1042836781793280.0
1045586878791680.0
1043386801192960.0
1046136898191360.0
1043936820592640.0
1046686917591040.0
1044486839992320.0
1047236936990720.0
1045036859392000.0
1047786956390400.0
1045586878791680.0
1048336975790080.0
1046136898191360.0
1048886995189760.0
1046686917591040.0
1049437014589440.0
1047236936990720.0
1049987033989120.0
1047786956390400.0
1050537053388800.0
1048336975790080.0
1051087072788480.0
1048886995189760.0
1051637092188160.0
1049437014589440.0
1052187111587840.0
1049987033989120.0
1052737130987520.0
1050537053388800.0
1053287150387200.0
1051087072788480.0
1053837169786880.0
1051637092188160.0
1054387189186560.0
1052187111587840.0
1054937208586240.0
1052737130987520.0
1055487227985920.0
1053287150387200.0
1056037247385600.0
1053837169786880.0
1056587266785280.0
1054387189186560.0
1057137286184960.0
1054937208586240.0
1057687305584640.0
1055487227985920.0
1058237324984320.0
1056037247385600.0
1058787344384000.0
1056587266785280.01059337363783680.0

1059887383183360.0
1057137286184960.0
1060437402583040.0
1057687305584640.0
1060987421982720.0
1058237324984320.0
1061537441382400.0
1058787344384000.0
1062087460782080.0
1062637480181760.0
1059337363783680.0
1063187499581440.0
1059887383183360.0
1063737518981120.0
1060437402583040.0
1064287538380800.0
1060987421982720.0
1064837557780480.0
1061537441382400.0
1065387577180160.0
1062087460782080.0
1065937596579840.0
1062637480181760.0
1066487615979520.0
1063187499581440.0
1067037635379200.0
1063737518981120.0
1067587654778880.0
1068137674178560.01064287538380800.0

1068687693578240.01064837557780480.0

1065387577180160.0
1069237712977920.0
1065937596579840.0
1069787732377600.0
1066487615979520.0
1070337751777280.0
1067037635379200.0
1070887771176960.0
1067587654778880.0
1071437790576640.0
1068137674178560.0
1071987809976320.0
1068687693578240.0
1072537829376000.0
1069237712977920.0
1073087848775680.0
1069787732377600.0
1073637868175360.0
1070337751777280.0
1074187887575040.0
1070887771176960.0
1074737906974720.0
1071437790576640.0
1075287926374400.0
1071987809976320.0
1075837945774080.0
1072537829376000.0
1073087848775680.0
1076387965173760.0
1073637868175360.0
1076937984573440.0
1074187887575040.0
1077488003973120.0
1074737906974720.0
1078038023372800.0
1075287926374400.0
1078588042772480.0
1075837945774080.0
1079138062172160.0
1076387965173760.0
1079688081571840.0
1076937984573440.0
1080238100971520.0
1077488003973120.0
1080788120371200.0
1078038023372800.0
1081338139770880.0
1078588042772480.0
1081888159170560.01079138062172160.0

1079688081571840.0
1082438178570240.0
1080238100971520.0
1082988197969920.0
1080788120371200.0
1083538217369600.0
1081338139770880.0
1084088236769280.0
1081888159170560.0
1084638256168960.0
1082438178570240.0
1085188275568640.0
1082988197969920.0
1085738294968320.0
1083538217369600.0
1086288314368000.0
1084088236769280.0
1086838333767680.0
1084638256168960.0
1087388353167360.0
1085188275568640.0
1087938372567040.0
1085738294968320.01088488391966720.0

1089038411366400.01086288314368000.0

1086838333767680.0
1089588430766080.0
1087388353167360.0
1090138450165760.0
1087938372567040.0
1090688469565440.0
1088488391966720.0
1091238488965120.0
1089038411366400.0
1091788508364800.0
1089588430766080.01092338527764480.0

1092888547164160.0
1090138450165760.0
1093438566563840.01090688469565440.0

1093988585963520.0
1091238488965120.0
1094538605363200.0
1091788508364800.0
1095088624762880.0
1092338527764480.0
1095638644162560.0
1092888547164160.0
1096188663562240.0
1093438566563840.0
1096738682961920.0
1093988585963520.0
1097288702361600.0
1094538605363200.0
1097838721761280.0
1095088624762880.0
1098388741160960.0
1095638644162560.0
1098938760560640.0
1096188663562240.0
1099488779960320.0
1096738682961920.0
1100038799360000.0
1097288702361600.0
1100588818759680.0
1097838721761280.0
1101138838159360.0
1098388741160960.0
1101688857559040.0
1098938760560640.0
1102238876958720.0
1099488779960320.0
1102788896358400.0
1100038799360000.0
1103338915758080.0
1100588818759680.0
1103888935157760.0
1101138838159360.0
1104438954557440.0
1101688857559040.0
1104988973957120.01102238876958720.0

1102788896358400.0
1105538993356800.0
1103338915758080.0
1106089012756480.0
1103888935157760.0
1106639032156160.0
1104438954557440.0
1107189051555840.0
1104988973957120.0
1107739070955520.0
1105538993356800.0
1108289090355200.01106089012756480.0

1108839109754880.0
1106639032156160.0
1109389129154560.0
1107189051555840.0
1109939148554240.0
1107739070955520.0
1110489167953920.0
1108289090355200.0
1111039187353600.0
1108839109754880.0
1111589206753280.0
1109389129154560.0
1112139226152960.0
1109939148554240.0
1112689245552640.0
1113239264952320.0
1110489167953920.0
1113789284352000.0
1111039187353600.0
1114339303751680.0
1111589206753280.0
1114889323151360.0
1112139226152960.0
1115439342551040.0
1112689245552640.0
1115989361950720.0
1113239264952320.0
1116539381350400.0
1113789284352000.0
1117089400750080.0
1114339303751680.0
1117639420149760.0
1114889323151360.0
1118189439549440.0
1115439342551040.0
1118739458949120.0
1115989361950720.0
1119289478348800.0
1116539381350400.0
1119839497748480.0
1117089400750080.0
1120389517148160.0
1117639420149760.0
1120939536547840.0
1118189439549440.0
1121489555947520.0
1118739458949120.0
1122039575347200.0
1119289478348800.0
1122589594746880.0
1119839497748480.0
1123139614146560.0
1120389517148160.0
1123689633546240.0
1120939536547840.0
1124239652945920.0
1121489555947520.0
1124789672345600.0
1122039575347200.0
1125339691745280.0
1122589594746880.0
1125889711144960.0
1123139614146560.0
1126439730544640.0
1123689633546240.0
1126989749944320.0
1124239652945920.0
1127539769344000.01124789672345600.0

1125339691745280.0
1128089788743680.0
1125889711144960.0
1128639808143360.0
1126439730544640.0
1129189827543040.0
1126989749944320.0
1129739846942720.0
1127539769344000.0
1130289866342400.0
1128089788743680.0
1130839885742080.0
1128639808143360.0
1131389905141760.0
1131939924541440.01129189827543040.0

1129739846942720.0
1132489943941120.0
1130289866342400.0
1133039963340800.0
1130839885742080.0
1133589982740480.0
1131389905141760.0
1134140002140160.0
1131939924541440.0
1134690021539840.0
1132489943941120.0
1135240040939520.0
1133039963340800.0
1135790060339200.0
1133589982740480.0
1136340079738880.0
1134140002140160.0
1136890099138560.0
1134690021539840.0
1135240040939520.0
1137440118538240.0
1135790060339200.0
1137990137937920.0
1136340079738880.0
1138540157337600.0
1136890099138560.0
1139090176737280.0
1137440118538240.0
1139640196136960.0
1140190215536640.0
1137990137937920.0
1140740234936320.0
1138540157337600.0
1141290254336000.0
1139090176737280.0
1141840273735680.0
1139640196136960.0
1142390293135360.0
1140190215536640.0
1142940312535040.0
1143490331934720.0
1140740234936320.0
1144040351334400.0
1141290254336000.0
1144590370734080.0
1141840273735680.0
1145140390133760.0
1142390293135360.0
1145690409533440.0
1142940312535040.01146240428933120.0

1143490331934720.0
1146790448332800.0
1144040351334400.0
1147340467732480.0
1144590370734080.0
1147890487132160.0
1145140390133760.0
1148440506531840.0
1145690409533440.0
1148990525931520.0
1146240428933120.0
1149540545331200.0
1146790448332800.0
1150090564730880.0
1147340467732480.0
1150640584130560.0
1147890487132160.0
1151190603530240.0
1148440506531840.0
1151740622929920.0
1148990525931520.0
1152290642329600.0
1149540545331200.0
1152840661729280.0
1150090564730880.0
1153390681128960.0
1150640584130560.0
1153940700528640.0
1151190603530240.0
1154490719928320.0
1151740622929920.0
1155040739328000.0
1152290642329600.0
1155590758727680.0
1152840661729280.0
1156140778127360.0
1153390681128960.0
1156690797527040.0
1153940700528640.0
1157240816926720.0
1154490719928320.0
1157790836326400.0
1155040739328000.0
1158340855726080.0
1155590758727680.0
1158890875125760.0
1156140778127360.0
1159440894525440.0
1156690797527040.0
1159990913925120.0
1160540933324800.01157240816926720.0

1157790836326400.0
1161090952724480.0
1158340855726080.0
1161640972124160.0
1158890875125760.0
1162190991523840.0
1159440894525440.0
1162741010923520.0
1159990913925120.0
1163291030323200.0
1160540933324800.0
1163841049722880.0
1161090952724480.0
1164391069122560.0
1164941088522240.0
1161640972124160.0
1162190991523840.0
1165491107921920.0
1162741010923520.0
1166041127321600.0
1163291030323200.0
1166591146721280.0
1163841049722880.0
1167141166120960.0
1164391069122560.0
1167691185520640.0
1168241204920320.0
1164941088522240.0
1168791224320000.0
1165491107921920.0
1169341243719680.0
1166041127321600.0
1169891263119360.0
1166591146721280.0
1170441282519040.0
1167141166120960.0
1170991301918720.0
1167691185520640.0
1171541321318400.0
1168241204920320.0
1172091340718080.0
1168791224320000.0
1172641360117760.0
1169341243719680.0
1173191379517440.0
1169891263119360.0
1173741398917120.0
1170441282519040.0
1174291418316800.0
1170991301918720.0
1174841437716480.0
1171541321318400.0
1175391457116160.0
1172091340718080.0
1175941476515840.0
1176491495915520.01172641360117760.0

1173191379517440.0
1177041515315200.0
1173741398917120.0
1177591534714880.0
1174291418316800.0
1178141554114560.0
1174841437716480.0
1178691573514240.0
1175391457116160.0
1179241592913920.0
1175941476515840.0
1179791612313600.0
1176491495915520.0
1180341631713280.0
1177041515315200.0
1180891651112960.0
1177591534714880.0
1181441670512640.0
1178141554114560.0
1181991689912320.01178691573514240.0

1179241592913920.0
1182541709312000.0
1179791612313600.0
1183091728711680.0
1180341631713280.0
1183641748111360.0
1180891651112960.0
1184191767511040.0
1181441670512640.0
1184741786910720.0
1181991689912320.0
1185291806310400.0
1182541709312000.0
1185841825710080.0
1183091728711680.0
1186391845109760.0
1183641748111360.0
1186941864509440.0
1184191767511040.0
1187491883909120.0
1188041903308800.01184741786910720.0

1188591922708480.0
1185291806310400.0
1189141942108160.0
1185841825710080.0
1189691961507840.0
1186391845109760.0
1190241980907520.0
1186941864509440.0
1190792000307200.0
1187491883909120.0
1191342019706880.0
1188041903308800.0
1191892039106560.0
1188591922708480.0
1192442058506240.0
1189141942108160.0
1192992077905920.0
1189691961507840.0
1193542097305600.0
1190241980907520.0
1194092116705280.0
1190792000307200.0
1194642136104960.0
1191342019706880.0
1195192155504640.0
1191892039106560.0
1195742174904320.0
1192442058506240.0
1196292194304000.0
1192992077905920.0
1196842213703680.0
1193542097305600.0
1197392233103360.0
1194092116705280.0
1197942252503040.0
1194642136104960.0
1198492271902720.0
1195192155504640.0
1199042291302400.0
1195742174904320.0
1199592310702080.0
1196292194304000.0
1200142330101760.0
1196842213703680.0
1200692349501440.0
1197392233103360.0
1201242368901120.0
1197942252503040.0
1201792388300800.0
1198492271902720.0
1202342407700480.0
1199042291302400.0
1202892427100160.0
1199592310702080.0
1203442446499840.0
1200142330101760.0
1203992465899520.0
1200692349501440.0
1204542485299200.0
1201242368901120.0
1205092504698880.0
1201792388300800.0
1205642524098560.0
1202342407700480.0
1206192543498240.0
1202892427100160.0
1206742562897920.0
1203442446499840.0
1207292582297600.0
1203992465899520.0
1207842601697280.0
1204542485299200.0
1208392621096960.0
1205092504698880.01208942640496640.0

1209492659896320.0
1205642524098560.0
1210042679296000.0
1206192543498240.0
1210592698695680.0
1206742562897920.0
1211142718095360.0
1207292582297600.0
1211692737495040.0
1207842601697280.0
1212242756894720.0
1208392621096960.0
1212792776294400.0
1208942640496640.0
1213342795694080.0
1209492659896320.0
1213892815093760.0
1210042679296000.0
1214442834493440.0
1210592698695680.0
1214992853893120.0
1211142718095360.0
1215542873292800.0
1211692737495040.0
1216092892692480.0
1212242756894720.0
1216642912092160.0
1212792776294400.0
1217192931491840.0
1213342795694080.0
1217742950891520.0
1213892815093760.0
1218292970291200.0
1214442834493440.0
1218842989690880.0
1214992853893120.0
1219393009090560.0
1215542873292800.0
1219943028490240.0
1216092892692480.0
1220493047889920.01216642912092160.0

1217192931491840.0
1221043067289600.0
1221593086689280.0
1222143106088960.0
1222693125488640.0
1217742950891520.0
1223243144888320.0
1218292970291200.0
1223793164288000.0
1218842989690880.0
1224343183687680.0
1219393009090560.0
1224893203087360.0
1219943028490240.0
1225443222487040.01220493047889920.0

1221043067289600.0
1225993241886720.0
1221593086689280.0
1226543261286400.0
1222143106088960.0
1227093280686080.0
1222693125488640.0
1227643300085760.0
1223243144888320.0
1228193319485440.0
1223793164288000.0
1228743338885120.0
1224343183687680.0
1229293358284800.0
1224893203087360.0
1229843377684480.0
1225443222487040.0
1230393397084160.01225993241886720.0

1226543261286400.01230943416483840.0

1231493435883520.0
1227093280686080.0
1232043455283200.0
1227643300085760.0
1232593474682880.0
1228193319485440.0
1233143494082560.0
1228743338885120.0
1233693513482240.0
1234243532881920.0
1229293358284800.0
1234793552281600.0
1229843377684480.0
1235343571681280.0
1230393397084160.0
1235893591080960.0
1230943416483840.0
1236443610480640.0
1231493435883520.0
1236993629880320.0
1232043455283200.0
1237543649280000.0
1238093668679680.0
1232593474682880.0
1238643688079360.0
1233143494082560.0
1239193707479040.0
1233693513482240.0
1239743726878720.0
1234243532881920.0
1240293746278400.0
1234793552281600.0
1235343571681280.0
1240843765678080.0
1235893591080960.0
1241393785077760.0
1241943804477440.01236443610480640.0

1242493823877120.0
1236993629880320.0
1243043843276800.0
1237543649280000.0
1243593862676480.0
1238093668679680.0
1244143882076160.0
1238643688079360.0
1244693901475840.0
1239193707479040.0
1245243920875520.0
1239743726878720.0
1245793940275200.0
1240293746278400.0
1246343959674880.0
1240843765678080.0
1246893979074560.0
1241393785077760.0
1247443998474240.0
1241943804477440.0
1247994017873920.0
1242493823877120.0
1248544037273600.0
1249094056673280.0
1243043843276800.0
1249644076072960.0
1243593862676480.0
1250194095472640.0
1244143882076160.0
1250744114872320.0
1244693901475840.0
1251294134272000.0
1245243920875520.0
1251844153671680.0
1245793940275200.0
1252394173071360.0
1246343959674880.0
1252944192471040.0
1253494211870720.01246893979074560.0

1247443998474240.0
1254044231270400.0
1247994017873920.0
1254594250670080.0
1248544037273600.0
1255144270069760.0
1249094056673280.0
1255694289469440.0
1249644076072960.0
1256244308869120.0
1250194095472640.0
1256794328268800.0
1250744114872320.0
1257344347668480.0
1251294134272000.0
1257894367068160.0
1251844153671680.0
1258444386467840.0
1252394173071360.0
1258994405867520.0
1252944192471040.0
1259544425267200.0
1253494211870720.0
1260094444666880.0
1254044231270400.0
1260644464066560.0
1254594250670080.0
1261194483466240.0
1255144270069760.0
1261744502865920.0
1262294522265600.01255694289469440.0

1256244308869120.0
1262844541665280.0
1256794328268800.0
1263394561064960.0
1257344347668480.0
1263944580464640.0
1257894367068160.0
1264494599864320.0
1258444386467840.0
1265044619264000.0
1258994405867520.0
1265594638663680.0
1259544425267200.0
1266144658063360.0
1260094444666880.0
1266694677463040.0
1260644464066560.0
1267244696862720.0
1261194483466240.0
1267794716262400.0
1261744502865920.0
1268344735662080.01262294522265600.0

1262844541665280.0
1268894755061760.0
1263394561064960.0
1269444774461440.0
1263944580464640.0
1269994793861120.0
1264494599864320.0
1270544813260800.0
1265044619264000.0
1271094832660480.0
1265594638663680.0
1271644852060160.0
1266144658063360.0
1272194871459840.0
1266694677463040.0
1272744890859520.0
1267244696862720.0
1273294910259200.0
1267794716262400.0
1273844929658880.0
1268344735662080.0
1268894755061760.0
1274394949058560.0
1269444774461440.0
1274944968458240.0
1269994793861120.0
1275494987857920.0
1270544813260800.0
1276045007257600.0
1271094832660480.0
1276595026657280.0
1271644852060160.0
1277145046056960.0
1277695065456640.0
1272194871459840.0
1278245084856320.0
1272744890859520.0
1278795104256000.0
1273294910259200.0
1279345123655680.0
1273844929658880.0
1279895143055360.0
1274394949058560.0
1280445162455040.0
1274944968458240.0
1280995181854720.0
1275494987857920.0
1281545201254400.0
1282095220654080.0
1276045007257600.0
1282645240053760.0
1276595026657280.0
1283195259453440.0
1277145046056960.0
1283745278853120.0
1277695065456640.0
1278245084856320.0
1284295298252800.0
1278795104256000.0
1284845317652480.0
1279345123655680.0
1285395337052160.0
1279895143055360.0
1285945356451840.0
1280445162455040.0
1286495375851520.0
1280995181854720.0
1287045395251200.0
1281545201254400.0
1287595414650880.0
1282095220654080.0
1288145434050560.0
1282645240053760.0
1288695453450240.0
1283195259453440.0
1289245472849920.0
1283745278853120.0
1289795492249600.0
1284295298252800.0
1290345511649280.0
1284845317652480.0
1290895531048960.0
1285395337052160.0
1291445550448640.0
1285945356451840.0
1291995569848320.0
1286495375851520.0
1292545589248000.0
1287045395251200.0
1293095608647680.0
1287595414650880.0
1293645628047360.0
1288145434050560.0
1294195647447040.0
1294745666846720.01288695453450240.0

1289245472849920.0
1295295686246400.0
1295845705646080.0
1289795492249600.0
1296395725045760.0
1290345511649280.0
1296945744445440.0
1290895531048960.0
1297495763845120.0
1291445550448640.0
1298045783244800.0
1291995569848320.0
1298595802644480.0
1292545589248000.0
1299145822044160.0
1293095608647680.0
1299695841443840.0
1293645628047360.0
1300245860843520.0
1294195647447040.0
1300795880243200.0
1294745666846720.0
1301345899642880.0
1295295686246400.0
1301895919042560.0
1295845705646080.0
1302445938442240.0
1296395725045760.0
1302995957841920.0
1296945744445440.0
1303545977241600.0
1297495763845120.01304095996641280.0

1304646016040960.0
1298045783244800.0
1305196035440640.0
1298595802644480.0
1305746054840320.0
1299145822044160.0
1306296074240000.01299695841443840.0

1300245860843520.0
1306846093639680.0
1300795880243200.0
1307396113039360.0
1301345899642880.0
1307946132439040.0
1301895919042560.0
1308496151838720.0
1302445938442240.0
1309046171238400.0
1302995957841920.0
1309596190638080.0
1303545977241600.0
1310146210037760.0
1304095996641280.0
1310696229437440.0
1304646016040960.0
1311246248837120.0
1305196035440640.0
1311796268236800.0
1305746054840320.0
1312346287636480.0
1306296074240000.0
1312896307036160.0
1306846093639680.0
1313446326435840.0
1307396113039360.0
1313996345835520.0
1307946132439040.0
1314546365235200.0
1308496151838720.0
1315096384634880.0
1309046171238400.0
1315646404034560.0
1309596190638080.0
1316196423434240.0
1310146210037760.0
1316746442833920.0
1310696229437440.0
1317296462233600.0
1311246248837120.0
1317846481633280.0
1311796268236800.0
1318396501032960.0
1312346287636480.0
1318946520432640.0
1312896307036160.0
1319496539832320.0
1313446326435840.0
1320046559232000.0
1313996345835520.0
1320596578631680.0
1314546365235200.0
1321146598031360.0
1315096384634880.0
1321696617431040.0
1315646404034560.0
1322246636830720.0
1316196423434240.0
1322796656230400.0
1316746442833920.0
1323346675630080.0
1317296462233600.0
1323896695029760.0
1317846481633280.0
1324446714429440.01318396501032960.0

1318946520432640.0
1324996733829120.0
1319496539832320.0
1325546753228800.0
1320046559232000.0
1326096772628480.0
1320596578631680.0
1326646792028160.0
1321146598031360.0
1327196811427840.0
1321696617431040.0
1327746830827520.0
1322246636830720.0
1328296850227200.0
1322796656230400.0
1328846869626880.0
1323346675630080.0
1329396889026560.0
1323896695029760.0
1329946908426240.0
1324446714429440.0
1330496927825920.0
1324996733829120.0
1331046947225600.0
1325546753228800.0
1331596966625280.0
1326096772628480.0
1332146986024960.0
1326646792028160.0
1332697005424640.0
1333247024824320.01327196811427840.0

1333797044224000.0
1327746830827520.0
1334347063623680.0
1328296850227200.0
1334897083023360.0
1328846869626880.0
1335447102423040.0
1329396889026560.0
1335997121822720.0
1329946908426240.0
1336547141222400.0
1330496927825920.0
1337097160622080.0
1331046947225600.0
1337647180021760.0
1331596966625280.0
1338197199421440.0
1332146986024960.0
1338747218821120.0
1332697005424640.0
1339297238220800.0
1339847257620480.01333247024824320.0

1333797044224000.0
1340397277020160.0
1334347063623680.0
1340947296419840.0
1334897083023360.0
1341497315819520.0
1335447102423040.0
1342047335219200.0
1342597354618880.01335997121822720.0

1336547141222400.0
1343147374018560.0
1337097160622080.0
1343697393418240.0
1337647180021760.0
1344247412817920.0
1338197199421440.0
1344797432217600.0
1338747218821120.0
1345347451617280.0
1339297238220800.0
1345897471016960.0
1339847257620480.0
1346447490416640.0
1340397277020160.0
1346997509816320.0
1340947296419840.01347547529216000.0

1348097548615680.0
1341497315819520.0
1348647568015360.0
1342047335219200.0
1349197587415040.0
1342597354618880.0
1349747606814720.0
1343147374018560.0
1350297626214400.0
1343697393418240.0
1350847645614080.0
1344247412817920.0
1351397665013760.0
1344797432217600.0
1351947684413440.0
1345347451617280.0
1352497703813120.0
1345897471016960.0
1353047723212800.0
1346447490416640.0
1353597742612480.0
1346997509816320.0
1354147762012160.0
1347547529216000.0
1354697781411840.0
1348097548615680.0
1355247800811520.0
1348647568015360.0
1355797820211200.0
1349197587415040.0
1356347839610880.0
1349747606814720.0
1356897859010560.0
1350297626214400.0
1357447878410240.0
1350847645614080.0
1357997897809920.0
1351397665013760.0
1358547917209600.0
1351947684413440.0
1359097936609280.0
1352497703813120.0
1359647956008960.0
1353047723212800.0
1360197975408640.0
1353597742612480.0
1360747994808320.0
1354147762012160.0
1361298014208000.0
1354697781411840.0
1361848033607680.0
1355247800811520.0
1362398053007360.0
1355797820211200.0
1362948072407040.0
1356347839610880.0
1363498091806720.0
1356897859010560.0
1364048111206400.0
1357447878410240.0
1364598130606080.0
1357997897809920.0
1365148150005760.0
1358547917209600.0
1365698169405440.0
1359097936609280.0
1366248188805120.0
1359647956008960.0
1366798208204800.0
1360197975408640.0
1367348227604480.0
1360747994808320.0
1367898247004160.0
1361298014208000.0
1368448266403840.0
1361848033607680.0
1362398053007360.0
1368998285803520.0
1362948072407040.0
1369548305203200.0
1363498091806720.0
1370098324602880.0
1364048111206400.0
1370648344002560.0
1364598130606080.0
1371198363402240.0
1365148150005760.0
1371748382801920.0
1365698169405440.0
1372298402201600.0
1366248188805120.0
1372848421601280.0
1373398441000960.0
1366798208204800.0
1373948460400640.0
1367348227604480.0
1374498479800320.0
1367898247004160.0
1375048499200000.0
1368448266403840.0
1375598518599680.0
1368998285803520.0
1376148537999360.0
1369548305203200.0
1376698557399040.0
1370098324602880.0
1377248576798720.0
1370648344002560.0
1377798596198400.0
1371198363402240.0
1378348615598080.0
1371748382801920.0
1378898634997760.0
1372298402201600.0
1379448654397440.0
1372848421601280.0
1379998673797120.0
1373398441000960.0
1380548693196800.01373948460400640.0

1374498479800320.0
1381098712596480.0
1375048499200000.0
1381648731996160.0
1375598518599680.0
1382198751395840.0
1376148537999360.0
1382748770795520.0
1376698557399040.0
1383298790195200.0
1377248576798720.0
1383848809594880.0
1377798596198400.0
1384398828994560.0
1378348615598080.0
1384948848394240.0
1385498867793920.0
1378898634997760.0
1386048887193600.0
1379448654397440.0
1386598906593280.0
1379998673797120.0
1387148925992960.0
1380548693196800.0
1387698945392640.0
1381098712596480.0
1388248964792320.0
1381648731996160.0
1388798984192000.0
1382198751395840.0
1389349003591680.0
1382748770795520.0
1389899022991360.0
1383298790195200.0
1390449042391040.0
1383848809594880.0
1390999061790720.0
1384398828994560.0
1391549081190400.0
1384948848394240.0
1385498867793920.0
1392099100590080.0
1386048887193600.0
1392649119989760.0
1386598906593280.0
1393199139389440.0
1387148925992960.0
1393749158789120.0
1387698945392640.0
1394299178188800.0
1388248964792320.0
1394849197588480.0
1388798984192000.0
1395399216988160.0
1389349003591680.0
1395949236387840.0
1389899022991360.0
1396499255787520.0
1390449042391040.0
1397049275187200.0
1390999061790720.0
1397599294586880.0
1391549081190400.0
1398149313986560.01392099100590080.0

1392649119989760.0
1398699333386240.0
1399249352785920.01393199139389440.0

1399799372185600.01393749158789120.0

1394299178188800.0
1400349391585280.0
1394849197588480.0
1400899410984960.0
1395399216988160.0
1401449430384640.0
1395949236387840.0
1401999449784320.0
1396499255787520.0
1402549469184000.0
1397049275187200.0
1403099488583680.0
1397599294586880.0
1403649507983360.0
1398149313986560.0
1404199527383040.0
1398699333386240.0
1404749546782720.0
1399249352785920.0
1405299566182400.0
1399799372185600.0
1405849585582080.0
1400349391585280.0
1406399604981760.0
1400899410984960.0
1406949624381440.0
1401449430384640.0
1407499643781120.0
1401999449784320.0
1402549469184000.0
1408049663180800.0
1403099488583680.0
1408599682580480.0
1403649507983360.0
1404199527383040.0
1409149701980160.0
1404749546782720.0
1409699721379840.0
1405299566182400.0
1410249740779520.0
1405849585582080.0
1410799760179200.0
1406399604981760.0
1411349779578880.0
1406949624381440.0
1411899798978560.0
1407499643781120.0
1412449818378240.0
1408049663180800.0
1412999837777920.0
1408599682580480.0
1413549857177600.0
1409149701980160.0
1414099876577280.0
1409699721379840.0
1414649895976960.0
1410249740779520.0
1415199915376640.0
1410799760179200.0
1415749934776320.0
1411349779578880.0
1416299954176000.0
1411899798978560.0
1416849973575680.0
1412449818378240.0
1417399992975360.0
1412999837777920.0
1417950012375040.0
1413549857177600.0
1418500031774720.0
1414099876577280.0
1419050051174400.0
1414649895976960.0
1419600070574080.0
1415199915376640.0
1420150089973760.0
1415749934776320.0
1420700109373440.0
1416299954176000.0
1421250128773120.0
1416849973575680.0
1421800148172800.0
1422350167572480.0
1417399992975360.01422900186972160.0

1423450206371840.0
1417950012375040.0
1424000225771520.0
1418500031774720.0
1424550245171200.0
1419050051174400.0
1425100264570880.0
1419600070574080.0
1425650283970560.0
1420150089973760.0
1426200303370240.0
1420700109373440.0
1426750322769920.0
1421250128773120.0
1427300342169600.0
1421800148172800.0
1427850361569280.0
1422350167572480.0
1428400380968960.0
1422900186972160.0
1428950400368640.0
1423450206371840.0
1429500419768320.0
1424000225771520.0
1430050439168000.0
1424550245171200.0
1430600458567680.0
1425100264570880.0
1431150477967360.0
1425650283970560.0
1431700497367040.0
1426200303370240.0
1432250516766720.0
1426750322769920.0
1432800536166400.0
1427300342169600.0
1433350555566080.0
1427850361569280.0
1433900574965760.0
1428400380968960.0
1434450594365440.0
1428950400368640.0
1435000613765120.0
1429500419768320.0
1435550633164800.0
1430050439168000.01436100652564480.0

1436650671964160.0
1430600458567680.0
1437200691363840.0
1431150477967360.0
1437750710763520.0
1431700497367040.0
1438300730163200.0
1432250516766720.0
1438850749562880.0
1432800536166400.0
1439400768962560.0
1433350555566080.0
1439950788362240.0
1433900574965760.0
1440500807761920.0
1441050827161600.0
1434450594365440.0
1441600846561280.0
1435000613765120.0
1442150865960960.0
1435550633164800.0
1442700885360640.0
1436100652564480.0
1443250904760320.0
1436650671964160.0
1443800924160000.0
1437200691363840.0
1444350943559680.0
1437750710763520.0
1444900962959360.0
1438300730163200.0
1445450982359040.0
1438850749562880.0
1446001001758720.0
1439400768962560.0
1446551021158400.0
1439950788362240.0
1447101040558080.0
1440500807761920.0
1447651059957760.0
1441050827161600.0
1448201079357440.0
1441600846561280.0
1448751098757120.0
1442150865960960.0
1449301118156800.0
1442700885360640.0
1449851137556480.0
1443250904760320.0
1450401156956160.0
1443800924160000.0
1450951176355840.0
1444350943559680.01451501195755520.0

1452051215155200.0
1444900962959360.0
1445450982359040.0
1452601234554880.0
1453151253954560.01446001001758720.0

1453701273354240.01446551021158400.0

1447101040558080.0
1454251292753920.0
1447651059957760.0
1454801312153600.0
1448201079357440.0
1455351331553280.0
1448751098757120.0
1455901350952960.0
1449301118156800.0
1456451370352640.0
1449851137556480.0
1457001389752320.0
1450401156956160.0
1457551409152000.0
1450951176355840.0
1458101428551680.0
1451501195755520.0
1458651447951360.0
1452051215155200.0
1459201467351040.0
1452601234554880.0
1459751486750720.0
1460301506150400.0
1453151253954560.0
1460851525550080.0
1453701273354240.0
1461401544949760.0
1454251292753920.0
1461951564349440.0
1454801312153600.0
1462501583749120.0
1455351331553280.0
1463051603148800.0
1455901350952960.0
1463601622548480.0
1456451370352640.0
1464151641948160.0
1457001389752320.0
1464701661347840.0
1457551409152000.0
1465251680747520.0
1458101428551680.0
1465801700147200.0
1458651447951360.0
1466351719546880.0
1459201467351040.0
1466901738946560.0
1459751486750720.0
1467451758346240.0
1460301506150400.0
1468001777745920.0
1460851525550080.0
1468551797145600.0
1461401544949760.0
1469101816545280.0
1461951564349440.0
1469651835944960.0
1462501583749120.0
1470201855344640.0
1463051603148800.0
1470751874744320.0
1463601622548480.0
1471301894144000.0
1464151641948160.01471851913543680.0

1472401932943360.0
1464701661347840.0
1472951952343040.0
1465251680747520.0
1473501971742720.0
1465801700147200.0
1474051991142400.0
1466351719546880.0
1474602010542080.0
1466901738946560.0
1475152029941760.0
1467451758346240.0
1475702049341440.0
1468001777745920.0
1476252068741120.0
1468551797145600.0
1476802088140800.0
1469101816545280.0
1477352107540480.0
1469651835944960.0
1470201855344640.0
1477902126940160.0
1470751874744320.0
1478452146339840.0
1471301894144000.0
1479002165739520.0
1471851913543680.0
1479552185139200.0
1472401932943360.0
1480102204538880.0
1472951952343040.0
1480652223938560.0
1473501971742720.0
1481202243338240.0
1474051991142400.0
1481752262737920.0
1474602010542080.0
1482302282137600.0
1475152029941760.0
1482852301537280.0
1475702049341440.0
1483402320936960.0
1476252068741120.0
1483952340336640.0
1476802088140800.0
1484502359736320.0
1477352107540480.0
1485052379136000.0
1477902126940160.0
1478452146339840.0
1485602398535680.0
1479002165739520.0
1486152417935360.0
1479552185139200.01486702437335040.0

1487252456734720.0
1480102204538880.0
1487802476134400.0
1480652223938560.0
1488352495534080.0
1488902514933760.01481202243338240.0

1481752262737920.0
1489452534333440.0
1482302282137600.0
1490002553733120.0
1482852301537280.0
1490552573132800.0
1483402320936960.0
1491102592532480.0
1483952340336640.0
1491652611932160.0
1484502359736320.0
1492202631331840.0
1485052379136000.0
1492752650731520.0
1485602398535680.0
1493302670131200.0
1486152417935360.0
1493852689530880.0
1486702437335040.0
1494402708930560.0
1487252456734720.0
1494952728330240.01487802476134400.0

1488352495534080.0
1495502747729920.0
1488902514933760.0
1496052767129600.0
1489452534333440.0
1496602786529280.0
1490002553733120.0
1497152805928960.0
1490552573132800.0
1497702825328640.0
1491102592532480.0
1498252844728320.0
1491652611932160.0
1498802864128000.0
1492202631331840.0
1499352883527680.0
1492752650731520.0
1499902902927360.0
1493302670131200.0
1500452922327040.0
1493852689530880.0
1501002941726720.0
1494402708930560.0
1501552961126400.0
1494952728330240.0
1502102980526080.0
1495502747729920.0
1502652999925760.0
1496052767129600.0
1503203019325440.0
1496602786529280.0
1503753038725120.0
1497152805928960.0
1504303058124800.0
1497702825328640.0
1504853077524480.01498252844728320.0

1498802864128000.0
1505403096924160.0
1505953116323840.0
1499352883527680.0
1506503135723520.0
1499902902927360.0
1507053155123200.0
1500452922327040.0
1507603174522880.0
1501002941726720.0
1501552961126400.0
1508153193922560.0
1508703213322240.01502102980526080.0

1502652999925760.0
1509253232721920.0
1503203019325440.0
1509803252121600.0
1503753038725120.0
1510353271521280.0
1504303058124800.0
1510903290920960.0
1504853077524480.0
1511453310320640.0
1505403096924160.0
1512003329720320.0
1505953116323840.0
1512553349120000.0
1506503135723520.0
1513103368519680.01507053155123200.0

1513653387919360.0
1507603174522880.0
1514203407319040.0
1508153193922560.0
1514753426718720.0
1508703213322240.0
1515303446118400.0
1509253232721920.0
1515853465518080.0
1509803252121600.0
1516403484917760.0
1510353271521280.0
1516953504317440.0
1510903290920960.0
1517503523717120.0
1511453310320640.0
1518053543116800.0
1512003329720320.0
1518603562516480.0
1512553349120000.0
1519153581916160.0
1513103368519680.0
1519703601315840.0
1513653387919360.0
1520253620715520.0
1514203407319040.0
1520803640115200.0
1514753426718720.0
1521353659514880.0
1515303446118400.0
1521903678914560.0
1515853465518080.0
1522453698314240.0
1516403484917760.0
1523003717713920.0
1516953504317440.0
1523553737113600.0
1517503523717120.0
1518053543116800.0
1524103756513280.0
1518603562516480.0
1524653775912960.0
1519153581916160.0
1525203795312640.0
1519703601315840.0
1525753814712320.0
1520253620715520.0
1526303834112000.0
1520803640115200.0
1526853853511680.0
1521353659514880.0
1527403872911360.0
1521903678914560.0
1527953892311040.0
1522453698314240.0
1528503911710720.0
1523003717713920.0
1529053931110400.0
1523553737113600.0
1529603950510080.0
1524103756513280.0
1530153969909760.0
1524653775912960.0
1530703989309440.0
1525203795312640.0
1531254008709120.0
1525753814712320.0
1531804028108800.0
1526303834112000.0
1532354047508480.0
1526853853511680.0
1532904066908160.01527403872911360.0

1527953892311040.0
1533454086307840.0
1528503911710720.0
1534004105707520.0
1529053931110400.0
1534554125107200.0
1529603950510080.0
1535104144506880.0
1530153969909760.0
1535654163906560.0
1530703989309440.0
1536204183306240.0
1531254008709120.0
1536754202705920.0
1531804028108800.0
1537304222105600.0
1532354047508480.0
1537854241505280.0
1532904066908160.0
1538404260904960.0
1533454086307840.0
1534004105707520.0
1538954280304640.0
1534554125107200.0
1539504299704320.0
1535104144506880.0
1540054319104000.0
1535654163906560.0
1540604338503680.0
1536204183306240.0
1541154357903360.01536754202705920.0

1537304222105600.0
1541704377303040.0
1537854241505280.0
1542254396702720.0
1538404260904960.0
1542804416102400.0
1538954280304640.0
1543354435502080.0
1539504299704320.0
1543904454901760.0
1540054319104000.0
1544454474301440.0
1540604338503680.0
1545004493701120.01541154357903360.0

1541704377303040.0
1545554513100800.0
1542254396702720.0
1546104532500480.0
1542804416102400.0
1546654551900160.0
1543354435502080.0
1547204571299840.0
1547754590699520.0
1543904454901760.0
1544454474301440.0
1545004493701120.0
1548304610099200.0
1545554513100800.0
1548854629498880.0
1546104532500480.0
1549404648898560.0
1546654551900160.0
1549954668298240.0
1547204571299840.0
1550504687697920.0
1547754590699520.0
1551054707097600.0
1548304610099200.0
1551604726497280.0
1548854629498880.0
1552154745896960.0
1552704765296640.0
1549404648898560.0
1553254784696320.0
1549954668298240.0
1553804804096000.0
1550504687697920.0
1554354823495680.0
1551054707097600.0
1554904842895360.0
1551604726497280.0
1555454862295040.0
1552154745896960.0
1556004881694720.0
1552704765296640.0
1556554901094400.0
1553254784696320.01557104920494080.0

1553804804096000.0
1557654939893760.0
1554354823495680.0
1558204959293440.0
1554904842895360.0
1558754978693120.0
1555454862295040.0
1559304998092800.0
1556004881694720.0
1559855017492480.0
1556554901094400.0
1560405036892160.0
1557104920494080.0
1560955056291840.0
1557654939893760.0
1561505075691520.0
1558204959293440.0
1562055095091200.0
1558754978693120.0
1562605114490880.0
1559304998092800.0
1563155133890560.0
1559855017492480.0
1563705153290240.0
1560405036892160.0
1564255172689920.0
1560955056291840.0
1564805192089600.0
1561505075691520.0
1565355211489280.0
1565905230888960.01562055095091200.0

1562605114490880.0
1566455250288640.0
1563155133890560.0
1567005269688320.0
1563705153290240.0
1567555289088000.0
1564255172689920.0
1568105308487680.0
1564805192089600.0
1568655327887360.0
1565355211489280.0
1569205347287040.0
1565905230888960.0
1569755366686720.0
1566455250288640.0
1570305386086400.0
1567005269688320.0
1570855405486080.0
1567555289088000.0
1571405424885760.0
1568105308487680.0
1571955444285440.0
1568655327887360.0
1572505463685120.0
1569205347287040.0
1573055483084800.0
1569755366686720.0
1573605502484480.01570305386086400.0

1570855405486080.0
1574155521884160.0
1574705541283840.0
1571405424885760.0
1575255560683520.0
1571955444285440.0
1575805580083200.0
1572505463685120.0
1576355599482880.0
1573055483084800.0
1576905618882560.01573605502484480.0

1577455638282240.0
1574155521884160.0
1578005657681920.0
1574705541283840.0
1578555677081600.0
1575255560683520.0
1579105696481280.0
1575805580083200.0
1579655715880960.0
1576355599482880.0
1580205735280640.0
1576905618882560.0
1580755754680320.0
1577455638282240.0
1581305774080000.0
1581855793479680.0
1578005657681920.0
1582405812879360.0
1578555677081600.0
1582955832279040.0
1579105696481280.0
1583505851678720.0
1584055871078400.0
1579655715880960.0
1584605890478080.0
1580205735280640.0
1585155909877760.0
1580755754680320.0
1585705929277440.0
1586255948677120.0
1581305774080000.0
1581855793479680.0
1586805968076800.0
1582405812879360.0
1587355987476480.0
1582955832279040.0
1587906006876160.0
1583505851678720.0
1588456026275840.0
1584055871078400.0
1589006045675520.0
1584605890478080.0
1589556065075200.0
1585155909877760.0
1590106084474880.0
1585705929277440.0
1590656103874560.0
1586255948677120.0
1591206123274240.0
1586805968076800.0
1591756142673920.0
1587355987476480.0
1592306162073600.0
1587906006876160.0
1592856181473280.0
1588456026275840.0
1593406200872960.0
1589006045675520.0
1593956220272640.0
1589556065075200.0
1594506239672320.0
1590106084474880.0
1595056259072000.0
1590656103874560.0
1595606278471680.0
1591206123274240.0
1596156297871360.0
1591756142673920.0
1596706317271040.0
1592306162073600.0
1597256336670720.0
1592856181473280.0
1597806356070400.0
1593406200872960.0
1598356375470080.0
1593956220272640.0
1598906394869760.0
1594506239672320.0
1599456414269440.0
1595056259072000.0
1600006433669120.0
1595606278471680.0
1600556453068800.0
1596156297871360.0
1601106472468480.0
1596706317271040.0
1601656491868160.0
1597256336670720.0
1602206511267840.0
1597806356070400.0
1602756530667520.0
1598356375470080.0
1603306550067200.0
1598906394869760.0
1603856569466880.0
1599456414269440.0
1604406588866560.0
1600006433669120.0
1604956608266240.0
1600556453068800.0
1605506627665920.0
1601106472468480.0
1606056647065600.0
1601656491868160.0
1606606666465280.0
1602206511267840.0
1607156685864960.0
1602756530667520.0
1607706705264640.0
1603306550067200.0
1608256724664320.0
1603856569466880.0
1608806744064000.0
1604406588866560.0
1609356763463680.0
1604956608266240.0
1609906782863360.0
1605506627665920.0
1610456802263040.0
1606056647065600.0
1611006821662720.0
1606606666465280.0
1611556841062400.0
1607156685864960.0
1612106860462080.0
1607706705264640.0
1612656879861760.0
1608256724664320.0
1608806744064000.01613206899261440.0

1613756918661120.0
1609356763463680.0
1614306938060800.0
1609906782863360.0
1610456802263040.0
1614856957460480.0
1611006821662720.0
1615406976860160.0
1611556841062400.01615956996259840.0

1612106860462080.0
1616507015659520.0
1612656879861760.01617057035059200.0

1617607054458880.0
1613206899261440.0
1618157073858560.0
1613756918661120.0
1618707093258240.0
1614306938060800.0
1619257112657920.0
1614856957460480.0
1619807132057600.0
1615406976860160.0
1620357151457280.0
1615956996259840.0
1620907170856960.0
1616507015659520.0
1617057035059200.0
1621457190256640.0
1617607054458880.0
1622007209656320.0
1618157073858560.0
1622557229056000.0
1618707093258240.0
1623107248455680.0
1619257112657920.0
1623657267855360.0
1619807132057600.01624207287255040.0

1624757306654720.01620357151457280.0

1625307326054400.0
1620907170856960.0
1625857345454080.0
1621457190256640.0
1626407364853760.01622007209656320.0

1622557229056000.0
1626957384253440.0
1623107248455680.0
1627507403653120.0
1623657267855360.0
1628057423052800.0
1624207287255040.0
1628607442452480.0
1624757306654720.0
1629157461852160.0
1625307326054400.0
1629707481251840.0
1625857345454080.0
1630257500651520.0
1626407364853760.0
1630807520051200.0
1626957384253440.0
1631357539450880.0
1627507403653120.0
1628057423052800.0
1631907558850560.0
1628607442452480.0
1632457578250240.0
1629157461852160.0
1633007597649920.0
1633557617049600.0
1629707481251840.0
1634107636449280.0
1630257500651520.0
1634657655848960.0
1630807520051200.0
1635207675248640.0
1631357539450880.0
1635757694648320.0
1631907558850560.0
1636307714048000.0
1632457578250240.0
1636857733447680.0
1633007597649920.0
1637407752847360.0
1633557617049600.0
1637957772247040.0
1634107636449280.0
1634657655848960.01638507791646720.0

1635207675248640.0
1639057811046400.01635757694648320.0

1639607830446080.0
1636307714048000.0
1640157849845760.0
1636857733447680.0
1640707869245440.0
1637407752847360.0
1641257888645120.0
1637957772247040.0
1641807908044800.0
1638507791646720.0
1642357927444480.0
1639057811046400.0
1642907946844160.0
1639607830446080.0
1643457966243840.0
1640157849845760.0
1644007985643520.0
1640707869245440.0
1644558005043200.0
1641257888645120.0
1645108024442880.0
1641807908044800.0
1645658043842560.0
1642357927444480.0
1646208063242240.0
1642907946844160.0
1646758082641920.0
1643457966243840.0
1647308102041600.0
1644007985643520.0
1647858121441280.0
1644558005043200.0
1648408140840960.0
1645108024442880.0
1645658043842560.0
1648958160240640.0
1649508179640320.0
1646208063242240.0
1650058199040000.0
1646758082641920.0
1650608218439680.0
1647308102041600.0
1651158237839360.0
1647858121441280.0
1651708257239040.0
1648408140840960.0
1652258276638720.0
1648958160240640.0
1652808296038400.0
1649508179640320.0
1653358315438080.0
1650058199040000.0
1653908334837760.0
1654458354237440.0
1650608218439680.0
1655008373637120.0
1651158237839360.0
1655558393036800.0
1651708257239040.0
1656108412436480.0
1652258276638720.0
1652808296038400.0
1656658431836160.0
1653358315438080.0
1657208451235840.0
1653908334837760.0
1657758470635520.0
1654458354237440.0
1658308490035200.0
1655008373637120.0
1658858509434880.0
1655558393036800.01659408528834560.0

1659958548234240.0
1656108412436480.0
1660508567633920.0
1656658431836160.0
1661058587033600.0
1657208451235840.0
1661608606433280.0
1657758470635520.0
1662158625832960.0
1658308490035200.0
1662708645232640.0
1658858509434880.0
1663258664632320.0
1659408528834560.0
1663808684032000.0
1659958548234240.0
1664358703431680.0
1660508567633920.0
1664908722831360.0
1661058587033600.0
1665458742231040.0
1661608606433280.0
1666008761630720.0
1666558781030400.0
1662158625832960.0
1667108800430080.0
1662708645232640.0
1667658819829760.0
1663258664632320.0
1668208839229440.0
1663808684032000.0
1668758858629120.0
1664358703431680.0
1669308878028800.0
1664908722831360.0
1669858897428480.0
1665458742231040.0
1670408916828160.0
1666008761630720.0
1670958936227840.0
1666558781030400.0
1671508955627520.0
1667108800430080.0
1672058975027200.0
1667658819829760.0
1672608994426880.0
1668208839229440.0
1673159013826560.0
1668758858629120.0
1673709033226240.0
1669308878028800.0
1674259052625920.0
1669858897428480.0
1674809072025600.0
1670408916828160.0
1675359091425280.0
1670958936227840.0
1675909110824960.0
1671508955627520.0
1676459130224640.0
1672058975027200.0
1677009149624320.0
1672608994426880.0
1677559169024000.0
1673159013826560.0
1678109188423680.0
1673709033226240.0
1678659207823360.0
1674259052625920.0
1679209227223040.0
1674809072025600.0
1679759246622720.0
1675359091425280.0
1680309266022400.0
1675909110824960.0
1680859285422080.0
1676459130224640.0
1681409304821760.0
1677009149624320.0
1681959324221440.0
1677559169024000.0
1682509343621120.0
1678109188423680.0
1683059363020800.0
1678659207823360.0
1683609382420480.0
1679209227223040.0
1684159401820160.0
1684709421219840.0
1679759246622720.0
1685259440619520.0
1680309266022400.0
1685809460019200.0
1680859285422080.0
1686359479418880.0
1681409304821760.0
1686909498818560.0
1681959324221440.0
1687459518218240.0
1682509343621120.0
1688009537617920.0
1683059363020800.0
1688559557017600.0
1683609382420480.0
1689109576417280.0
1684159401820160.0
1689659595816960.0
1684709421219840.0
1690209615216640.0
1685259440619520.0
1690759634616320.0
1685809460019200.0
1691309654016000.0
1686359479418880.0
1691859673415680.0
1686909498818560.0
1692409692815360.0
1687459518218240.01692959712215040.0

1693509731614720.0
1688009537617920.0
1694059751014400.0
1688559557017600.0
1694609770414080.0
1689109576417280.0
1695159789813760.0
1689659595816960.0
1695709809213440.0
1690209615216640.0
1696259828613120.0
1690759634616320.0
1696809848012800.0
1691309654016000.0
1697359867412480.0
1691859673415680.0
1697909886812160.0
1692409692815360.0
1698459906211840.0
1692959712215040.0
1699009925611520.0
1693509731614720.0
1699559945011200.0
1694059751014400.0
1700109964410880.0
1694609770414080.0
1700659983810560.0
1695159789813760.0
1701210003210240.0
1695709809213440.0
1701760022609920.0
1696259828613120.0
1702310042009600.0
1696809848012800.0
1702860061409280.0
1697359867412480.0
1703410080808960.0
1697909886812160.0
1703960100208640.0
1698459906211840.0
1704510119608320.0
1699009925611520.0
1705060139008000.0
1699559945011200.0
1705610158407680.0
1700109964410880.0
1706160177807360.0
1700659983810560.0
1706710197207040.0
1701210003210240.0
1707260216606720.0
1701760022609920.0
1707810236006400.0
1702310042009600.0
1708360255406080.0
1702860061409280.0
1708910274805760.0
1703410080808960.0
1709460294205440.0
1703960100208640.0
1710010313605120.0
1704510119608320.0
1710560333004800.0
1705060139008000.0
1711110352404480.0
1705610158407680.0
1711660371804160.0
1706160177807360.0
1712210391203840.0
1706710197207040.0
1712760410603520.0
1707260216606720.0
1713310430003200.0
1707810236006400.0
1713860449402880.0
1708360255406080.0
1714410468802560.0
1708910274805760.0
1714960488202240.0
1709460294205440.0
1715510507601920.0
1710010313605120.0
1716060527001600.0
1710560333004800.0
1716610546401280.0
1711110352404480.0
1717160565800960.0
1711660371804160.0
1717710585200640.0
1712210391203840.0
1718260604600320.0
1712760410603520.0
1718810624000000.0
1713310430003200.0
1719360643399680.0
1713860449402880.0
1719910662799360.0
1714410468802560.0
1720460682199040.0
1714960488202240.0
1721010701598720.0
1715510507601920.0
1721560720998400.0
1716060527001600.0
1722110740398080.0
1716610546401280.0
1722660759797760.0
1717160565800960.0
1723210779197440.0
1717710585200640.0
1723760798597120.0
1718260604600320.0
1724310817996800.0
1718810624000000.0
1724860837396480.0
1719360643399680.0
1725410856796160.0
1719910662799360.0
1725960876195840.0
1726510895595520.01720460682199040.0

1727060914995200.0
1721010701598720.0
1727610934394880.0
1721560720998400.0
1728160953794560.0
1722110740398080.0
1728710973194240.0
1722660759797760.0
1729260992593920.0
1723210779197440.0
1729811011993600.0
1730361031393280.01723760798597120.0

1724310817996800.0
1730911050792960.0
1724860837396480.01731461070192640.0

1725410856796160.0
1732011089592320.0
1725960876195840.0
1732561108992000.0
1726510895595520.0
1733111128391680.0
1727060914995200.0
1733661147791360.0
1727610934394880.0
1734211167191040.0
1728160953794560.0
1734761186590720.0
1728710973194240.0
1735311205990400.0
1729260992593920.0
1735861225390080.0
1729811011993600.0
1736411244789760.0
1730361031393280.0
1736961264189440.0
1730911050792960.0
1737511283589120.0
1731461070192640.0
1738061302988800.0
1732011089592320.0
1738611322388480.0
1739161341788160.0
1732561108992000.0
1739711361187840.0
1733111128391680.0
1740261380587520.0
1733661147791360.0
1740811399987200.0
1734211167191040.0
1741361419386880.0
1734761186590720.0
1741911438786560.0
1735311205990400.0
1742461458186240.0
1735861225390080.0
1743011477585920.0
1736411244789760.0
1743561496985600.0
1736961264189440.0
1744111516385280.0
1737511283589120.0
1744661535784960.0
1738061302988800.0
1745211555184640.0
1738611322388480.0
1745761574584320.0
1739161341788160.0
1746311593984000.0
1739711361187840.0
1746861613383680.0
1740261380587520.0
1747411632783360.0
1740811399987200.0
1741361419386880.0
1747961652183040.0
1741911438786560.0
1748511671582720.0
1742461458186240.0
1749061690982400.0
1743011477585920.0
1749611710382080.0
1743561496985600.0
1750161729781760.0
1744111516385280.0
1750711749181440.0
1744661535784960.0
1751261768581120.0
1745211555184640.0
1751811787980800.0
1745761574584320.0
1752361807380480.0
1746311593984000.0
1752911826780160.0
1746861613383680.0
1753461846179840.0
1747411632783360.0
1754011865579520.0
1747961652183040.0
1754561884979200.0
1748511671582720.01755111904378880.0

1749061690982400.0
1755661923778560.0
1749611710382080.0
1756211943178240.0
1750161729781760.0
1756761962577920.0
1750711749181440.0
1751261768581120.0
1757311981977600.0
1751811787980800.0
1757862001377280.0
1752361807380480.0
1758412020776960.0
1752911826780160.0
1758962040176640.0
1753461846179840.0
1759512059576320.0
1754011865579520.0
1760062078976000.0
1754561884979200.01760612098375680.0

1761162117775360.01755111904378880.0

1761712137175040.0
1755661923778560.0
1762262156574720.0
1756211943178240.0
1762812175974400.0
1756761962577920.0
1763362195374080.0
1757311981977600.0
1763912214773760.0
1757862001377280.0
1764462234173440.0
1758412020776960.0
1765012253573120.0
1758962040176640.01765562272972800.0

1759512059576320.0
1766112292372480.0
1760062078976000.0
1766662311772160.0
1760612098375680.0
1767212331171840.0
1761162117775360.0
1767762350571520.0
1761712137175040.0
1768312369971200.0
1762262156574720.0
1768862389370880.0
1762812175974400.0
1769412408770560.0
1763362195374080.0
1769962428170240.01763912214773760.0

1770512447569920.01764462234173440.0

1771062466969600.0
1765012253573120.0
1771612486369280.0
1765562272972800.0
1772162505768960.0
1766112292372480.0
1772712525168640.0
1766662311772160.0
1773262544568320.0
1767212331171840.0
1773812563968000.0
1767762350571520.0
1774362583367680.0
1768312369971200.0
1774912602767360.0
1768862389370880.0
1775462622167040.0
1769412408770560.0
1776012641566720.0
1769962428170240.0
1776562660966400.0
1770512447569920.0
1777112680366080.0
1771062466969600.0
1777662699765760.0
1778212719165440.01771612486369280.0

1778762738565120.01772162505768960.0

1779312757964800.01772712525168640.0

1773262544568320.0
1779862777364480.0
1773812563968000.0
1780412796764160.0
1774362583367680.0
1780962816163840.0
1774912602767360.0
1781512835563520.0
1775462622167040.0
1782062854963200.0
1776012641566720.0
1782612874362880.0
1776562660966400.0
1783162893762560.0
1777112680366080.0
1783712913162240.0
1777662699765760.0
1784262932561920.0
1778212719165440.0
1784812951961600.0
1778762738565120.0
1785362971361280.0
1779312757964800.0
1785912990760960.0
1779862777364480.0
1786463010160640.0
1780412796764160.0
1787013029560320.0
1780962816163840.0
1787563048960000.0
1781512835563520.0
1788113068359680.0
1782062854963200.0
1788663087759360.0
1782612874362880.0
1789213107159040.0
1783162893762560.0
1789763126558720.0
1790313145958400.01783712913162240.0

1784262932561920.0
1790863165358080.0
1784812951961600.0
1791413184757760.0
1785362971361280.0
1791963204157440.0
1792513223557120.0
1785912990760960.0
1793063242956800.0
1786463010160640.0
1793613262356480.0
1787013029560320.0
1794163281756160.0
1787563048960000.0
1794713301155840.0
1788113068359680.0
1795263320555520.0
1788663087759360.0
1795813339955200.0
1789213107159040.0
1796363359354880.0
1789763126558720.0
1796913378754560.01790313145958400.0

1797463398154240.01790863165358080.0

1791413184757760.0
1798013417553920.0
1791963204157440.0
1798563436953600.0
1792513223557120.0
1799113456353280.0
1793063242956800.0
1799663475752960.0
1800213495152640.01793613262356480.0

1794163281756160.0
1800763514552320.0
1794713301155840.0
1801313533952000.0
1795263320555520.0
1801863553351680.0
1795813339955200.0
1796363359354880.0
1802413572751360.0
1796913378754560.0
1802963592151040.0
1797463398154240.0
1803513611550720.0
1798013417553920.0
1804063630950400.0
1798563436953600.0
1804613650350080.0
1799113456353280.0
1805163669749760.0
1799663475752960.0
1805713689149440.0
1800213495152640.0
1806263708549120.0
1800763514552320.0
1806813727948800.0
1801313533952000.0
1807363747348480.0
1801863553351680.0
1807913766748160.0
1802413572751360.0
1802963592151040.01808463786147840.0

1809013805547520.0
1803513611550720.0
1809563824947200.0
1804063630950400.0
1810113844346880.0
1804613650350080.0
1810663863746560.0
1805163669749760.0
1811213883146240.0
1805713689149440.0
1811763902545920.0
1806263708549120.0
1812313921945600.0
1806813727948800.0
1812863941345280.0
1807363747348480.01813413960744960.0

1813963980144640.0
1807913766748160.0
1814513999544320.0
1808463786147840.0
1815064018944000.0
1809013805547520.0
1815614038343680.0
1809563824947200.0
1816164057743360.0
1810113844346880.0
1810663863746560.0
1816714077143040.0
1811213883146240.0
1817264096542720.0
1811763902545920.0
1817814115942400.0
1812313921945600.0
1818364135342080.0
1812863941345280.0
1818914154741760.0
1813413960744960.0
1819464174141440.0
1820014193541120.0
1813963980144640.0
1820564212940800.0
1814513999544320.0
1821114232340480.0
1815064018944000.0
1821664251740160.0
1815614038343680.0
1816164057743360.0
1822214271139840.0
1816714077143040.0
1822764290539520.01817264096542720.0

1823314309939200.0
1817814115942400.0
1823864329338880.0
1818364135342080.0
1824414348738560.0
1818914154741760.0
1824964368138240.0
1819464174141440.0
1825514387537920.01820014193541120.0

1826064406937600.0
1820564212940800.0
1826614426337280.0
1821114232340480.0
1827164445736960.0
1821664251740160.0
1827714465136640.0
1822214271139840.0
1828264484536320.0
1822764290539520.0
1828814503936000.0
1823314309939200.0
1829364523335680.0
1823864329338880.0
1829914542735360.0
1824414348738560.0
1830464562135040.0
1824964368138240.0
1831014581534720.0
1825514387537920.0
1831564600934400.0
1826064406937600.0
1832114620334080.0
1826614426337280.0
1832664639733760.0
1827164445736960.0
1833214659133440.0
1827714465136640.0
1833764678533120.0
1828264484536320.0
1834314697932800.0
1828814503936000.0
1834864717332480.0
1829364523335680.0
1835414736732160.0
1829914542735360.0
1835964756131840.0
1830464562135040.0
1836514775531520.0
1831014581534720.0
1837064794931200.0
1837614814330880.0
1831564600934400.0
1838164833730560.0
1832114620334080.0
1838714853130240.0
1832664639733760.0
1839264872529920.0
1833214659133440.0
1839814891929600.0
1833764678533120.0
1840364911329280.0
1834314697932800.0
1840914930728960.0
1834864717332480.0
1841464950128640.0
1835414736732160.0
1842014969528320.0
1835964756131840.0
1842564988928000.0
1836514775531520.01843115008327680.0

1843665027727360.0
1837064794931200.0
1844215047127040.0
1837614814330880.0
1844765066526720.0
1838164833730560.0
1845315085926400.0
1838714853130240.0
1845865105326080.0
1839264872529920.0
1846415124725760.0
1839814891929600.0
1846965144125440.0
1840364911329280.0
1840914930728960.0
1847515163525120.0
1841464950128640.0
1848065182924800.0
1842014969528320.0
1848615202324480.0
1842564988928000.0
1849165221724160.0
1843115008327680.0
1849715241123840.0
1850265260523520.0
1843665027727360.0
1850815279923200.0
1844215047127040.0
1851365299322880.0
1844765066526720.0
1851915318722560.0
1845315085926400.0
1852465338122240.0
1845865105326080.0
1853015357521920.0
1846415124725760.0
1853565376921600.0
1846965144125440.0
1854115396321280.0
1847515163525120.0
1854665415720960.0
1848065182924800.0
1855215435120640.0
1848615202324480.0
1855765454520320.0
1849165221724160.0
1856315473920000.0
1849715241123840.0
1856865493319680.0
1850265260523520.0
1857415512719360.0
1850815279923200.0
1857965532119040.0
1858515551518720.01851365299322880.0

1851915318722560.0
1859065570918400.0
1852465338122240.0
1859615590318080.0
1853015357521920.0
1860165609717760.0
1853565376921600.0
1860715629117440.0
1854115396321280.0
1861265648517120.0
1854665415720960.0
1861815667916800.0
1855215435120640.0
1862365687316480.0
1855765454520320.0
1862915706716160.0
1856315473920000.0
1863465726115840.0
1856865493319680.0
1864015745515520.0
1857415512719360.0
1864565764915200.0
1857965532119040.0
1865115784314880.0
1858515551518720.0
1865665803714560.0
1859065570918400.0
1866215823114240.0
1859615590318080.0
1866765842513920.0
1860165609717760.0
1867315861913600.0
1860715629117440.0
1867865881313280.0
1861265648517120.0
1861815667916800.0
1868415900712960.0
1868965920112640.0
1862365687316480.0
1869515939512320.0
1862915706716160.0
1870065958912000.0
1863465726115840.0
1870615978311680.0
1864015745515520.0
1871165997711360.0
1864565764915200.0
1871716017111040.0
1865115784314880.0
1872266036510720.0
1865665803714560.0
1872816055910400.0
1866215823114240.0
1873366075310080.0
1866765842513920.0
1873916094709760.0
1867315861913600.0
1867865881313280.0
1868415900712960.0
1874466114109440.0
1868965920112640.0
1875016133509120.0
1869515939512320.0
1875566152908800.0
1870065958912000.0
1876116172308480.0
1870615978311680.0
1876666191708160.0
1871165997711360.0
1877216211107840.0
1871716017111040.0
1877766230507520.0
1872266036510720.0
1878316249907200.0
1872816055910400.0
1878866269306880.0
1873366075310080.0
1879416288706560.0
1873916094709760.0
1879966308106240.0
1874466114109440.0
1880516327505920.0
1875016133509120.0
1881066346905600.0
1875566152908800.0
1881616366305280.0
1876116172308480.0
1882166385704960.0
1876666191708160.0
1882716405104640.0
1877216211107840.0
1883266424504320.0
1877766230507520.0
1883816443904000.0
1878316249907200.0
1884366463303680.01878866269306880.0

1879416288706560.0
1884916482703360.0
1879966308106240.0
1885466502103040.0
1880516327505920.01886016521502720.0

1886566540902400.0
1881066346905600.0
1887116560302080.0
1881616366305280.0
1887666579701760.0
1882166385704960.0
1888216599101440.0
1882716405104640.0
1888766618501120.0
1883266424504320.0
1889316637900800.0
1883816443904000.0
1889866657300480.0
1884366463303680.0
1890416676700160.0
1884916482703360.0
1890966696099840.0
1885466502103040.0
1891516715499520.01886016521502720.0

1892066734899200.0
1886566540902400.01892616754298880.0

1893166773698560.0
1887116560302080.0
1893716793098240.0
1887666579701760.0
1894266812497920.0
1888216599101440.0
1894816831897600.0
1888766618501120.0
1895366851297280.0
1889316637900800.0
1895916870696960.0
1889866657300480.01896466890096640.0

1890416676700160.0
1897016909496320.0
1890966696099840.0
1897566928896000.0
1891516715499520.0
1898116948295680.0
1892066734899200.0
1898666967695360.0
1892616754298880.0
1899216987095040.0
1893166773698560.0
1899767006494720.0
1893716793098240.0
1900317025894400.0
1894266812497920.0
1900867045294080.0
1894816831897600.0
1901417064693760.0
1895366851297280.0
1895916870696960.0
1901967084093440.0
1896466890096640.0
1902517103493120.0
1897016909496320.0
1903067122892800.0
1897566928896000.0
1903617142292480.0
1898116948295680.0
1904167161692160.0
1898666967695360.0
1899216987095040.0
1904717181091840.0
1899767006494720.0
1905267200491520.0
1900317025894400.0
1905817219891200.0
1900867045294080.0
1906367239290880.0
1901417064693760.0
1906917258690560.0
1901967084093440.0
1907467278090240.0
1902517103493120.0
1908017297489920.0
1903067122892800.0
1908567316889600.0
1903617142292480.0
1909117336289280.0
1904167161692160.0
1909667355688960.01904717181091840.0

1905267200491520.0
1910217375088640.0
1910767394488320.0
1905817219891200.0
1911317413888000.0
1906367239290880.0
1906917258690560.0
1911867433287680.0
1912417452687360.0
1907467278090240.0
1912967472087040.0
1908017297489920.0
1913517491486720.0
1908567316889600.0
1914067510886400.0
1909117336289280.0
1914617530286080.0
1909667355688960.0
1915167549685760.0
1910217375088640.0
1915717569085440.0
1910767394488320.0
1916267588485120.0
1911317413888000.0
1916817607884800.0
1911867433287680.0
1917367627284480.0
1912417452687360.0
1917917646684160.0
1912967472087040.0
1918467666083840.0
1913517491486720.0
1919017685483520.0
1914067510886400.0
1919567704883200.0
1914617530286080.0
1920117724282880.0
1915167549685760.0
1920667743682560.0
1915717569085440.0
1921217763082240.0
1921767782481920.0
1922317801881600.0
1916267588485120.0
1922867821281280.0
1916817607884800.0
1923417840680960.0
1917367627284480.0
1923967860080640.0
1917917646684160.0
1924517879480320.0
1918467666083840.0
1925067898880000.0
1919017685483520.0
1925617918279680.0
1919567704883200.0
1926167937679360.0
1920117724282880.0
1926717957079040.0
1920667743682560.0
1927267976478720.0
1921217763082240.0
1927817995878400.0
1921767782481920.0
1928368015278080.0
1922317801881600.01928918034677760.0

1929468054077440.0
1922867821281280.0
1930018073477120.0
1923417840680960.0
1930568092876800.0
1923967860080640.0
1931118112276480.0
1924517879480320.0
1931668131676160.0
1925067898880000.0
1932218151075840.0
1925617918279680.0
1932768170475520.0
1926167937679360.0
1933318189875200.0
1926717957079040.0
1933868209274880.0
1927267976478720.0
1934418228674560.0
1927817995878400.0
1934968248074240.0
1928368015278080.0
1935518267473920.0
1928918034677760.0
1936068286873600.0
1929468054077440.0
1936618306273280.0
1930018073477120.0
1937168325672960.0
1930568092876800.0
1937718345072640.0
1931118112276480.0
1938268364472320.0
1931668131676160.0
1938818383872000.0
1932218151075840.0
1939368403271680.0
1932768170475520.0
1939918422671360.0
1940468442071040.0
1933318189875200.0
1941018461470720.0
1933868209274880.0
1941568480870400.0
1934418228674560.0
1942118500270080.0
1934968248074240.0
1942668519669760.0
1935518267473920.0
1943218539069440.0
1936068286873600.0
1943768558469120.0
1936618306273280.0
1944318577868800.0
1937168325672960.0
1944868597268480.0
1937718345072640.0
1945418616668160.0
1938268364472320.0
1945968636067840.0
1938818383872000.0
1946518655467520.0
1939368403271680.0
1947068674867200.0
1939918422671360.0
1947618694266880.0
1940468442071040.0
1948168713666560.0
1941018461470720.0
1948718733066240.0
1941568480870400.0
1949268752465920.0
1942118500270080.0
1949818771865600.0
1942668519669760.0
1950368791265280.0
1943218539069440.0
1950918810664960.0
1943768558469120.0
1951468830064640.0
1944318577868800.0
1952018849464320.0
1944868597268480.0
1952568868864000.0
1945418616668160.0
1953118888263680.0
1945968636067840.0
1953668907663360.0
1946518655467520.0
1954218927063040.0
1947068674867200.0
1954768946462720.0
1947618694266880.0
1955318965862400.0
1948168713666560.0
1955868985262080.0
1948718733066240.0
1956419004661760.0
1949268752465920.0
1956969024061440.0
1949818771865600.0
1957519043461120.0
1950368791265280.0
1958069062860800.0
1950918810664960.0
1951468830064640.0
1958619082260480.0
1952018849464320.0
1959169101660160.0
1952568868864000.0
1959719121059840.0
1953118888263680.0
1960269140459520.0
1953668907663360.0
1960819159859200.0
1954218927063040.0
1961369179258880.0
1954768946462720.0
1961919198658560.0
1955318965862400.0
1962469218058240.0
1955868985262080.0
1963019237457920.0
1956419004661760.0
1963569256857600.0
1956969024061440.0
1964119276257280.0
1957519043461120.0
1964669295656960.01958069062860800.0

1965219315056640.0
1958619082260480.0
1965769334456320.0
1959169101660160.0
1966319353856000.0
1959719121059840.0
1966869373255680.0
1960269140459520.01967419392655360.0

1967969412055040.0
1960819159859200.0
1968519431454720.0
1961369179258880.0
1969069450854400.0
1961919198658560.0
1969619470254080.0
1962469218058240.0
1970169489653760.0
1963019237457920.0
1970719509053440.0
1963569256857600.0
1964119276257280.0
1971269528453120.0
1964669295656960.0
1971819547852800.0
1965219315056640.0
1972369567252480.0
1965769334456320.01972919586652160.0

1966319353856000.0
1973469606051840.0
1966869373255680.0
1974019625451520.0
1967419392655360.0
1974569644851200.0
1967969412055040.0
1975119664250880.0
1968519431454720.0
1975669683650560.0
1969069450854400.0
1976219703050240.0
1969619470254080.0
1976769722449920.0
1970169489653760.0
1977319741849600.0
1970719509053440.0
1977869761249280.01971269528453120.0

1971819547852800.0
1978419780648960.0
1972369567252480.01978969800048640.0

1972919586652160.0
1979519819448320.0
1973469606051840.0
1980069838848000.0
1974019625451520.0
1980619858247680.0
1974569644851200.0
1981169877647360.0
1975119664250880.0
1981719897047040.0
1975669683650560.0
1982269916446720.0
1976219703050240.0
1982819935846400.0
1976769722449920.0
1983369955246080.0
1977319741849600.0
1983919974645760.0
1977869761249280.0
1984469994045440.0
1978419780648960.0
1985020013445120.0
1978969800048640.0
1985570032844800.0
1979519819448320.0
1986120052244480.0
1980069838848000.0
1986670071644160.01980619858247680.0

1987220091043840.0
1981169877647360.0
1987770110443520.0
1981719897047040.0
1988320129843200.0
1982269916446720.0
1988870149242880.0
1982819935846400.0
1989420168642560.0
1983369955246080.0
1989970188042240.0
1983919974645760.0
1990520207441920.0
1984469994045440.0
1991070226841600.0
1985020013445120.0
1991620246241280.0
1985570032844800.0
1992170265640960.0
1986120052244480.0
1992720285040640.0
1986670071644160.0
1993270304440320.0
1987220091043840.0
1993820323840000.0
1987770110443520.0
1994370343239680.0
1988320129843200.0
1994920362639360.0
1988870149242880.0
1995470382039040.0
1996020401438720.01989420168642560.0

1989970188042240.0
1996570420838400.0
1990520207441920.0
1997120440238080.01991070226841600.0

1991620246241280.0
1997670459637760.0
1992170265640960.0
1998220479037440.0
1992720285040640.0
1998770498437120.0
1993270304440320.0
1999320517836800.0
1993820323840000.0
1999870537236480.0
1994370343239680.0
2000420556636160.0
1994920362639360.0
2000970576035840.0
1995470382039040.0
2001520595435520.0
1996020401438720.0
1996570420838400.0
2002070614835200.0
2002620634234880.0
1997120440238080.0
2003170653634560.01997670459637760.0

1998220479037440.0
2003720673034240.0
1998770498437120.0
2004270692433920.0
1999320517836800.0
2004820711833600.0
1999870537236480.0
2005370731233280.0
2000420556636160.0
2005920750632960.0
2000970576035840.0
2006470770032640.0
2001520595435520.0
2002070614835200.0
2007020789432320.0
2002620634234880.0
2007570808832000.0
2003170653634560.0
2008120828231680.0
2003720673034240.0
2008670847631360.0
2004270692433920.0
2009220867031040.0
2004820711833600.0
2009770886430720.0
2005370731233280.0
2010320905830400.0
2005920750632960.0
2010870925230080.0
2006470770032640.0
2011420944629760.0
2007020789432320.0
2007570808832000.0
2011970964029440.0
2008120828231680.0
2012520983429120.0
2008670847631360.0
2013071002828800.0
2009220867031040.0
2013621022228480.0
2009770886430720.0
2014171041628160.0
2010320905830400.0
2014721061027840.0
2010870925230080.0
2015271080427520.0
2011420944629760.0
2015821099827200.0
2011970964029440.0
2016371119226880.0
2012520983429120.02016921138626560.0

2013071002828800.0
2017471158026240.0
2013621022228480.0
2018021177425920.0
2014171041628160.0
2018571196825600.0
2014721061027840.0
2019121216225280.0
2015271080427520.0
2019671235624960.0
2015821099827200.0
2020221255024640.0
2016371119226880.0
2020771274424320.0
2016921138626560.0
2021321293824000.0
2017471158026240.0
2021871313223680.0
2018021177425920.0
2022421332623360.0
2018571196825600.0
2022971352023040.0
2019121216225280.0
2023521371422720.02019671235624960.0

2020221255024640.0
2024071390822400.0
2020771274424320.02024621410222080.0

2025171429621760.0
2021321293824000.0
2025721449021440.0
2021871313223680.0
2026271468421120.0
2022421332623360.0
2026821487820800.0
2022971352023040.0
2027371507220480.0
2023521371422720.0
2027921526620160.0
2024071390822400.0
2028471546019840.0
2024621410222080.0
2029021565419520.0
2025171429621760.0
2029571584819200.0
2025721449021440.0
2030121604218880.0
2026271468421120.0
2030671623618560.0
2026821487820800.0
2031221643018240.0
2027371507220480.0
2031771662417920.0
2027921526620160.0
2032321681817600.0
2028471546019840.0
2032871701217280.0
2029021565419520.0
2033421720616960.0
2029571584819200.0
2033971740016640.0
2030121604218880.0
2034521759416320.0
2030671623618560.0
2035071778816000.0
2031221643018240.0
2035621798215680.0
2031771662417920.0
2036171817615360.0
2032321681817600.0
2036721837015040.0
2032871701217280.0
2037271856414720.0
2033421720616960.0
2037821875814400.0
2033971740016640.0
2038371895214080.0
2038921914613760.0
2034521759416320.0
2039471934013440.0
2035071778816000.0
2040021953413120.0
2035621798215680.0
2040571972812800.0
2036171817615360.0
2041121992212480.0
2036721837015040.0
2041672011612160.0
2037271856414720.0
2042222031011840.0
2037821875814400.0
2042772050411520.0
2038371895214080.0
2043322069811200.0
2038921914613760.0
2043872089210880.0
2039471934013440.0
2044422108610560.0
2040021953413120.0
2044972128010240.0
2040571972812800.0
2045522147409920.0
2041121992212480.0
2046072166809600.0
2041672011612160.0
2046622186209280.0
2042222031011840.0
2047172205608960.0
2042772050411520.0
2047722225008640.0
2043322069811200.0
2048272244408320.0
2043872089210880.0
2048822263808000.0
2044422108610560.0
2044972128010240.0
2049372283207680.0
2049922302607360.0
2045522147409920.0
2050472322007040.0
2046072166809600.0
2051022341406720.0
2046622186209280.02051572360806400.0

2047172205608960.0
2052122380206080.0
2047722225008640.0
2052672399605760.0
2053222419005440.02048272244408320.0

2048822263808000.0
2053772438405120.0
2049372283207680.0
2054322457804800.0
2049922302607360.0
2054872477204480.0
2050472322007040.0
2055422496604160.0
2051022341406720.0
2055972516003840.0
2051572360806400.0
2056522535403520.0
2052122380206080.0
2057072554803200.0
2052672399605760.0
2057622574202880.0
2053222419005440.0
2058172593602560.02053772438405120.0

2054322457804800.0
2058722613002240.0
2054872477204480.0
2059272632401920.0
2055422496604160.0
2059822651801600.0
2055972516003840.0
2060372671201280.0
2056522535403520.0
2060922690600960.0
2057072554803200.0
2061472710000640.0
2057622574202880.0
2062022729400320.0
2058172593602560.0
2062572748800000.0
2058722613002240.0
2063122768199680.0
2059272632401920.0
2063672787599360.0
2059822651801600.0
2064222806999040.0
2060372671201280.0
2064772826398720.0
2060922690600960.0
2065322845798400.0
2061472710000640.0
2065872865198080.0
2062022729400320.0
2066422884597760.0
2062572748800000.0
2066972903997440.0
2063122768199680.0
2067522923397120.0
2063672787599360.0
2068072942796800.0
2064222806999040.0
2068622962196480.0
2064772826398720.0
2069172981596160.0
2065322845798400.0
2069723000995840.0
2065872865198080.0
2070273020395520.0
2066422884597760.0
2070823039795200.0
2066972903997440.0
2071373059194880.0
2067522923397120.0
2071923078594560.0
2068072942796800.0
2072473097994240.0
2068622962196480.0
2069172981596160.02073023117393920.0

2073573136793600.0
2069723000995840.0
2074123156193280.0
2070273020395520.0
2074673175592960.0
2070823039795200.0
2075223194992640.0
2071373059194880.0
2075773214392320.0
2071923078594560.0
2076323233792000.0
2072473097994240.0
2076873253191680.0
2073023117393920.0
2077423272591360.0
2073573136793600.0
2077973291991040.0
2074123156193280.0
2078523311390720.0
2074673175592960.0
2079073330790400.0
2075223194992640.0
2079623350190080.0
2075773214392320.0
2080173369589760.0
2076323233792000.0
2080723388989440.0
2076873253191680.0
2081273408389120.0
2077423272591360.0
2081823427788800.0
2077973291991040.0
2082373447188480.0
2078523311390720.0
2082923466588160.0
2079073330790400.0
2083473485987840.0
2079623350190080.0
2084023505387520.0
2080173369589760.0
2084573524787200.0
2080723388989440.0
2085123544186880.0
2081273408389120.0
2085673563586560.0
2081823427788800.0
2086223582986240.0
2082373447188480.0
2086773602385920.0
2082923466588160.0
2087323621785600.0
2083473485987840.0
2087873641185280.0
2084023505387520.0
2088423660584960.0
2084573524787200.0
2088973679984640.0
2085123544186880.0
2089523699384320.02085673563586560.0

2086223582986240.0
2090073718784000.0
2086773602385920.0
2090623738183680.0
2091173757583360.0
2087323621785600.0
2091723776983040.0
2087873641185280.0
2092273796382720.0
2088423660584960.0
2092823815782400.0
2088973679984640.0
2089523699384320.0
2093373835182080.0
2090073718784000.0
2093923854581760.0
2090623738183680.0
2094473873981440.0
2091173757583360.0
2095023893381120.0
2091723776983040.0
2095573912780800.0
2092273796382720.0
2096123932180480.0
2092823815782400.0
2096673951580160.02093373835182080.0

2093923854581760.0
2097223970979840.0
2094473873981440.0
2095023893381120.0
2097773990379520.0
2095573912780800.0
2098324009779200.0
2096123932180480.0
2098874029178880.0
2096673951580160.0
2099424048578560.0
2097223970979840.0
2099974067978240.0
2097773990379520.0
2100524087377920.0
2098324009779200.0
2101074106777600.0
2098874029178880.0
2101624126177280.0
2099424048578560.0
2102174145576960.0
2099974067978240.0
2102724164976640.0
2100524087377920.0
2103274184376320.0
2101074106777600.0
2103824203776000.0
2104374223175680.02101624126177280.0

2104924242575360.0
2102174145576960.0
2105474261975040.0
2102724164976640.0
2106024281374720.0
2103274184376320.0
2106574300774400.0
2103824203776000.0
2107124320174080.0
2104374223175680.0
2107674339573760.0
2104924242575360.0
2108224358973440.0
2105474261975040.0
2108774378373120.0
2106024281374720.0
2109324397772800.0
2106574300774400.0
2109874417172480.0
2107124320174080.0
2110424436572160.0
2107674339573760.0
2110974455971840.0
2108224358973440.0
2111524475371520.0
2108774378373120.0
2112074494771200.0
2109324397772800.0
2112624514170880.0
2109874417172480.02113174533570560.0

2110424436572160.0
2113724552970240.0
2110974455971840.0
2114274572369920.0
2111524475371520.0
2114824591769600.0
2112074494771200.0
2115374611169280.0
2112624514170880.0
2115924630568960.0
2116474649968640.0
2113174533570560.0
2117024669368320.0
2113724552970240.0
2117574688768000.0
2114274572369920.0
2118124708167680.0
2114824591769600.0
2118674727567360.0
2115374611169280.0
2119224746967040.0
2115924630568960.0
2119774766366720.0
2116474649968640.0
2120324785766400.0
2117024669368320.0
2120874805166080.0
2117574688768000.0
2121424824565760.02118124708167680.0

2118674727567360.0
2121974843965440.0
2119224746967040.0
2122524863365120.0
2119774766366720.0
2123074882764800.0
2120324785766400.0
2123624902164480.0
2120874805166080.0
2124174921564160.0
2121424824565760.0
2124724940963840.0
2121974843965440.0
2125274960363520.0
2122524863365120.0
2125824979763200.0
2123074882764800.0
2126374999162880.0
2123624902164480.0
2124174921564160.0
2126925018562560.0
2124724940963840.0
2127475037962240.0
2125274960363520.0
2128025057361920.0
2125824979763200.0
2128575076761600.0
2126374999162880.0
2129125096161280.0
2126925018562560.0
2129675115560960.0
2127475037962240.0
2130225134960640.0
2128025057361920.0
2130775154360320.0
2128575076761600.0
2131325173760000.0
2129125096161280.0
2131875193159680.0
2129675115560960.0
2132425212559360.0
2132975231959040.0
2130225134960640.0
2133525251358720.0
2130775154360320.0
2134075270758400.0
2131325173760000.0
2134625290158080.0
2131875193159680.0
2135175309557760.0
2132425212559360.0
2135725328957440.0
2136275348357120.02132975231959040.0

2133525251358720.0
2136825367756800.0
2134075270758400.0
2137375387156480.0
2134625290158080.0
2137925406556160.0
2135175309557760.0
2138475425955840.0
2135725328957440.0
2139025445355520.0
2136275348357120.0
2139575464755200.0
2136825367756800.0
2140125484154880.0
2137375387156480.0
2140675503554560.0
2137925406556160.0
2141225522954240.0
2138475425955840.0
2141775542353920.0
2139025445355520.0
2142325561753600.0
2139575464755200.0
2142875581153280.0
2140125484154880.0
2143425600552960.0
2140675503554560.0
2143975619952640.0
2141225522954240.0
2144525639352320.0
2141775542353920.0
2142325561753600.0
2145075658752000.0
2142875581153280.0
2145625678151680.0
2143425600552960.0
2146175697551360.0
2143975619952640.0
2146725716951040.0
2144525639352320.0
2147275736350720.0
2145075658752000.0
2147825755750400.0
2145625678151680.0
2148375775150080.0
2146175697551360.0
2148925794549760.0
2146725716951040.0
2149475813949440.0
2147275736350720.0
2150025833349120.0
2150575852748800.02147825755750400.0

2148375775150080.0
2151125872148480.0
2148925794549760.0
2151675891548160.0
2149475813949440.0
2152225910947840.02150025833349120.0

2150575852748800.0
2152775930347520.0
2151125872148480.0
2153325949747200.0
2151675891548160.0
2153875969146880.0
2152225910947840.0
2154425988546560.0
2152775930347520.0
2154976007946240.0
2153325949747200.0
2155526027345920.0
2153875969146880.0
2156076046745600.0
2154425988546560.0
2156626066145280.0
2154976007946240.0
2157176085544960.0
2155526027345920.0
2156076046745600.0
2157726104944640.0
2156626066145280.0
2158276124344320.0
2157176085544960.0
2158826143744000.0
2157726104944640.0
2159376163143680.0
2158276124344320.0
2159926182543360.0
2158826143744000.0
2160476201943040.0
2159376163143680.0
2161026221342720.0
2159926182543360.0
2161576240742400.0
2160476201943040.0
2162126260142080.0
2161026221342720.0
2162676279541760.0
2161576240742400.0
2163226298941440.0
2162126260142080.0
2163776318341120.0
2162676279541760.0
2164326337740800.0
2163226298941440.0
2164876357140480.0
2163776318341120.0
2165426376540160.0
2164326337740800.0
2165976395939840.0
2164876357140480.0
2166526415339520.0
2165426376540160.0
2167076434739200.0
2165976395939840.0
2167626454138880.0
2166526415339520.0
2168176473538560.0
2167076434739200.0
2168726492938240.0
2167626454138880.0
2169276512337920.0
2169826531737600.0
2168176473538560.0
2168726492938240.0
2170376551137280.0
2169276512337920.0
2170926570536960.0
2169826531737600.0
2171476589936640.0
2170376551137280.0
2172026609336320.0
2170926570536960.0
2172576628736000.0
2171476589936640.0
2173126648135680.0
2172026609336320.0
2173676667535360.0
2172576628736000.0
2174226686935040.0
2173126648135680.0
2174776706334720.0
2173676667535360.0
2175326725734400.0
2174226686935040.0
2175876745134080.0
2176426764533760.02174776706334720.0

2176976783933440.0
2175326725734400.0
2177526803333120.0
2175876745134080.0
2178076822732800.0
2176426764533760.0
2178626842132480.0
2176976783933440.0
2179176861532160.02177526803333120.0

2178076822732800.0
2179726880931840.0
2180276900331520.0
2178626842132480.0
2179176861532160.0
2180826919731200.0
2179726880931840.0
2181376939130880.0
2180276900331520.0
2181926958530560.0
2180826919731200.0
2182476977930240.0
2181376939130880.0
2183026997329920.0
2181926958530560.0
2183577016729600.0
2182476977930240.0
2184127036129280.0
2183026997329920.0
2184677055528960.0
2183577016729600.0
2185227074928640.0
2184127036129280.0
2185777094328320.0
2184677055528960.0
2186327113728000.0
2185227074928640.0
2186877133127680.0
2185777094328320.0
2187427152527360.0
2186327113728000.0
2187977171927040.0
2186877133127680.0
2188527191326720.0
2187427152527360.0
2189077210726400.0
2187977171927040.0
2189627230126080.0
2188527191326720.0
2190177249525760.0
2189077210726400.0
2190727268925440.0
2189627230126080.0
2191277288325120.0
2190177249525760.0
2191827307724800.0
2190727268925440.0
2192377327124480.0
2191277288325120.0
2192927346524160.0
2191827307724800.0
2193477365923840.0
2192377327124480.0
2194027385323520.0
2192927346524160.0
2194577404723200.0
2193477365923840.0
2195127424122880.0
2194027385323520.0
2195677443522560.0
2194577404723200.0
2196227462922240.0
2195127424122880.0
2196777482321920.0
2195677443522560.0
2197327501721600.0
2196227462922240.0
2197877521121280.0
2196777482321920.0
2198427540520960.0
2197327501721600.0
2198977559920640.0
2197877521121280.0
2199527579320320.02198427540520960.0

2200077598720000.02198977559920640.0

2199527579320320.0
2200627618119680.0
2200077598720000.0
2201177637519360.0
2200627618119680.0
2201727656919040.0
2201177637519360.0
2202277676318720.0
2201727656919040.0
2202827695718400.0
2203377715118080.0
2202277676318720.0
2203927734517760.0
2204477753917440.02202827695718400.0

2203377715118080.0
2205027773317120.0
2203927734517760.0
2205577792716800.0
2204477753917440.0
2206127812116480.0
2205027773317120.0
2206677831516160.0
2205577792716800.0
2207227850915840.0
2206127812116480.0
2207777870315520.0
2206677831516160.0
2208327889715200.0
2207227850915840.0
2208877909114880.0
2207777870315520.0
2209427928514560.0
2208327889715200.0
2209977947914240.0
2208877909114880.0
2210527967313920.0
2209427928514560.0
2211077986713600.0
2209977947914240.0
2211628006113280.0
2210527967313920.0
2212178025512960.0
2211077986713600.0
2212728044912640.0
2211628006113280.0
2213278064312320.0
2212178025512960.0
2213828083712000.0
2212728044912640.0
2214378103111680.0
2213278064312320.0
2214928122511360.0
2213828083712000.0
2215478141911040.0
2216028161310720.02214378103111680.0

2216578180710400.0
2214928122511360.0
2217128200110080.0
2215478141911040.0
2217678219509760.02216028161310720.0

2216578180710400.0
2218228238909440.0
2217128200110080.0
2218778258309120.0
2217678219509760.0
2219328277708800.0
2218228238909440.0
2219878297108480.0
2218778258309120.0
2220428316508160.0
2219328277708800.0
2220978335907840.0
2219878297108480.0
2221528355307520.0
2220428316508160.0
2222078374707200.0
2220978335907840.0
2221528355307520.0
2222628394106880.0
2222078374707200.0
2223178413506560.0
2222628394106880.0
2223728432906240.0
2223178413506560.0
2224278452305920.0
2223728432906240.0
2224828471705600.0
2224278452305920.0
2225378491105280.0
2224828471705600.0
2225378491105280.0
2225928510504960.0
2225928510504960.0
2226478529904640.0
2226478529904640.0
2227028549304320.0
2227028549304320.0
2227578568704000.0
2227578568704000.0
2228128588103680.0
2228128588103680.0
2228678607503360.0
2228678607503360.0
2229228626903040.0
2229228626903040.0
2229778646302720.0
2229778646302720.0
2230328665702400.0
2230328665702400.0
2230878685102080.0
2230878685102080.0
2231428704501760.0
2231428704501760.0
2231978723901440.0
2231978723901440.0
2232528743301120.0
2232528743301120.0
2233078762700800.0
2233078762700800.0
2233628782100480.0
2233628782100480.0
2234178801500160.0
2234178801500160.0
2234728820899840.0
2234728820899840.0
2235278840299520.0
2235278840299520.0
2235828859699200.0
2235828859699200.0
2236378879098880.0
2236378879098880.0
2236928898498560.0
2236928898498560.0
2237478917898240.0
2237478917898240.0
2238028937297920.0
2238028937297920.0
2238578956697600.0
2238578956697600.0
2239128976097280.0
2239678995496960.0
2239128976097280.0
2240229014896640.0
2239678995496960.0
2240779034296320.0
2240229014896640.0
2241329053696000.0
2240779034296320.0
2241879073095680.0
2241329053696000.0
2242429092495360.0
2241879073095680.0
2242979111895040.0
2242429092495360.0
2243529131294720.0
2242979111895040.0
2244079150694400.02243529131294720.0

2244629170094080.0
2244079150694400.0
2245179189493760.0
2244629170094080.0
2245729208893440.0
2245179189493760.0
2246279228293120.0
2245729208893440.0
2246829247692800.0
2246279228293120.0
2247379267092480.0
2246829247692800.0
2247929286492160.0
2247379267092480.0
2248479305891840.0
2247929286492160.0
2249029325291520.0
2248479305891840.0
2249579344691200.0
2249029325291520.0
2250129364090880.0
2249579344691200.0
2250679383490560.0
2250129364090880.0
2251229402890240.0
2250679383490560.0
2251779422289920.0
2251229402890240.0
2252329441689600.0
2252879461089280.0
2251779422289920.0
2253429480488960.0
2252329441689600.0
2253979499888640.0
2252879461089280.0
2254529519288320.0
2253429480488960.0
2255079538688000.0
2253979499888640.0
2255629558087680.0
2254529519288320.0
2256179577487360.0
2255079538688000.0
2256729596887040.0
2255629558087680.0
2257279616286720.0
2256179577487360.0
2256729596887040.0
2257829635686400.0
2257279616286720.0
2258379655086080.0
2257829635686400.0
2258929674485760.0
2258379655086080.0
2259479693885440.0
2258929674485760.0
2260029713285120.0
2259479693885440.0
2260579732684800.0
2260029713285120.0
2261129752084480.02260579732684800.0

2261129752084480.0
2261679771484160.0
2261679771484160.0
2262229790883840.0
2262229790883840.0
2262779810283520.0
2262779810283520.0
2263329829683200.0
2263329829683200.0
2263879849082880.0
2263879849082880.0
2264429868482560.0
2264429868482560.0
2264979887882240.0
2264979887882240.0
2265529907281920.0
2265529907281920.0
2266079926681600.02266079926681600.0

2266629946081280.0
2266629946081280.0
2267179965480960.0
2267179965480960.0
2267729984880640.0
2267729984880640.0
2268280004280320.0
2268280004280320.0
2268830023680000.0
2268830023680000.0
2269380043079680.0
2269380043079680.0
2269930062479360.0
2269930062479360.0
2270480081879040.0
2270480081879040.0
2271030101278720.0
2271030101278720.0
2271580120678400.0
2271580120678400.0
2272130140078080.0
2272130140078080.0
2272680159477760.0
2272680159477760.0
2273230178877440.0
2273230178877440.0
2273780198277120.0
2273780198277120.0
2274330217676800.0
2274330217676800.0
2274880237076480.0
2274880237076480.0
2275430256476160.0
2275430256476160.0
2275980275875840.0
2275980275875840.0
2276530295275520.0
2276530295275520.0
2277080314675200.0
2277080314675200.0
2277630334074880.0
2277630334074880.0
2278180353474560.0
2278180353474560.0
2278730372874240.0
2278730372874240.0
2279280392273920.0
2279280392273920.0
2279830411673600.0
2279830411673600.0
2280380431073280.0
2280380431073280.0
2280930450472960.0
2280930450472960.0
2281480469872640.0
2281480469872640.0
2282030489272320.0
2282030489272320.0
2282580508672000.0
2282580508672000.0
2283130528071680.0
2283130528071680.0
2283680547471360.02283680547471360.0

2284230566871040.0
2284230566871040.0
2284780586270720.0
2284780586270720.0
2285330605670400.0
2285330605670400.0
2285880625070080.0
2285880625070080.0
2286430644469760.0
2286430644469760.0
2286980663869440.0
2286980663869440.0
2287530683269120.0
2287530683269120.0
2288080702668800.0
2288080702668800.0
2288630722068480.02288630722068480.0

2289180741468160.0
2289180741468160.0
2289730760867840.0
2289730760867840.0
2290280780267520.0
2290280780267520.0
2290830799667200.0
2290830799667200.0
2291380819066880.0
2291380819066880.0
2291930838466560.0
2291930838466560.0
2292480857866240.0
2292480857866240.02293030877265920.0

2293030877265920.0
2293580896665600.0
2293580896665600.0
2294130916065280.0
2294130916065280.0
2294680935464960.0
2294680935464960.0
2295230954864640.0
2295230954864640.0
2295780974264320.0
2295780974264320.0
2296330993664000.0
2296330993664000.0
2296881013063680.0
2296881013063680.0
2297431032463360.02297431032463360.0

2297981051863040.0
2297981051863040.0
2298531071262720.0
2298531071262720.0
2299081090662400.0
2299081090662400.0
2299631110062080.0
2299631110062080.0
2300181129461760.0
2300181129461760.0
2300731148861440.0
2300731148861440.0
2301281168261120.0
2301281168261120.0
2301831187660800.0
2301831187660800.0
2302381207060480.0
2302381207060480.0
2302931226460160.0
2303481245859840.0
2302931226460160.0
2304031265259520.0
2303481245859840.0
2304581284659200.0
2304031265259520.0
2305131304058880.0
2304581284659200.0
2305681323458560.0
2305131304058880.0
2306231342858240.0
2305681323458560.0
2306781362257920.0
2306231342858240.0
2307331381657600.0
2306781362257920.0
2307881401057280.0
2307331381657600.0
2308431420456960.0
2307881401057280.0
2308981439856640.0
2308431420456960.0
2309531459256320.0
2308981439856640.0
2310081478656000.0
2309531459256320.0
2310631498055680.0
2310081478656000.0
2311181517455360.0
2310631498055680.0
2311731536855040.0
2311181517455360.02312281556254720.0

2312831575654400.0
2311731536855040.0
2313381595054080.0
2312281556254720.0
2313931614453760.0
2312831575654400.0
2314481633853440.0
2313381595054080.0
2315031653253120.0
2313931614453760.0
2315581672652800.0
2314481633853440.0
2316131692052480.0
2315031653253120.0
2316681711452160.0
2315581672652800.0
2317231730851840.0
2316131692052480.0
2317781750251520.0
2316681711452160.0
2318331769651200.0
2317231730851840.0
2318881789050880.0
2317781750251520.0
2319431808450560.0
2318331769651200.0
2319981827850240.0
2318881789050880.0
2320531847249920.0
2319431808450560.0
2321081866649600.0
2319981827850240.0
2321631886049280.0
2320531847249920.0
2322181905448960.0
2321081866649600.0
2322731924848640.0
2321631886049280.0
2323281944248320.0
2322181905448960.0
2323831963648000.0
2322731924848640.0
2324381983047680.0
2323281944248320.0
2324932002447360.0
2323831963648000.0
2324381983047680.0
2325482021847040.0
2324932002447360.0
2326032041246720.0
2325482021847040.0
2326582060646400.0
2326032041246720.0
2326582060646400.02327132080046080.0

2327682099445760.0
2327132080046080.0
2328232118845440.0
2327682099445760.0
2328782138245120.0
2328232118845440.0
2329332157644800.0
2328782138245120.0
2329882177044480.0
2329332157644800.0
2330432196444160.0
2329882177044480.0
2330432196444160.0
2330982215843840.0
2330982215843840.0
2331532235243520.0
2331532235243520.0
2332082254643200.0
2332082254643200.02332632274042880.0

2332632274042880.0
2333182293442560.0
2333182293442560.0
2333732312842240.0
2333732312842240.0
2334282332241920.0
2334282332241920.0
2334832351641600.0
2334832351641600.0
2335382371041280.0
2335382371041280.0
2335932390440960.0
2335932390440960.0
2336482409840640.0
2336482409840640.0
2337032429240320.0
2337032429240320.0
2337582448640000.0
2337582448640000.0
2338132468039680.0
2338132468039680.0
2338682487439360.0
2338682487439360.0
2339232506839040.0
2339232506839040.0
2339782526238720.0
2340332545638400.0
2339782526238720.0
2340882565038080.0
2340332545638400.0
2341432584437760.0
2340882565038080.0
2341982603837440.0
2341432584437760.0
2342532623237120.0
2341982603837440.0
2343082642636800.0
2342532623237120.0
2343082642636800.0
2343632662036480.0
2343632662036480.0
2344182681436160.0
2344182681436160.0
2344732700835840.0
2344732700835840.0
2345282720235520.0
2345282720235520.0
2345832739635200.0
2345832739635200.0
2346382759034880.0
2346382759034880.0
2346932778434560.0
2346932778434560.0
2347482797834240.0
2347482797834240.0
2348032817233920.0
2348032817233920.0
2348582836633600.0
2348582836633600.0
2349132856033280.0
2349132856033280.0
2349682875432960.0
2349682875432960.0
2350232894832640.0
2350232894832640.0
2350782914232320.0
2350782914232320.0
2351332933632000.0
2351332933632000.0
2351882953031680.0
2351882953031680.0
2352432972431360.0
2352432972431360.0
2352982991831040.0
2352982991831040.0
2353533011230720.0
2353533011230720.0
2354083030630400.0
2354083030630400.0
2354633050030080.0
2354633050030080.0
2355183069429760.0
2355183069429760.02355733088829440.0

2356283108229120.0
2355733088829440.0
2356833127628800.0
2356283108229120.0
2357383147028480.02356833127628800.0

2357933166428160.0
2357383147028480.0
2358483185827840.0
2357933166428160.0
2359033205227520.0
2358483185827840.0
2359583224627200.0
2359033205227520.0
2360133244026880.0
2359583224627200.0
2360683263426560.0
2360133244026880.0
2361233282826240.0
2360683263426560.0
2361783302225920.0
2361233282826240.0
2362333321625600.0
2361783302225920.0
2362883341025280.0
2362333321625600.0
2363433360424960.0
2362883341025280.0
2363983379824640.02363433360424960.0

2363983379824640.0
2364533399224320.0
2365083418624000.0
2364533399224320.0
2365633438023680.0
2366183457423360.0
2365083418624000.0
2365633438023680.0
2366733476823040.0
2366183457423360.0
2367283496222720.0
2366733476823040.0
2367833515622400.0
2367283496222720.0
2368383535022080.0
2367833515622400.0
2368933554421760.0
2368383535022080.0
2369483573821440.0
2368933554421760.0
2370033593221120.0
2369483573821440.0
2370583612620800.0
2370033593221120.0
2371133632020480.0
2370583612620800.0
2371683651420160.0
2371133632020480.0
2372233670819840.0
2371683651420160.0
2372783690219520.0
2372233670819840.0
2373333709619200.0
2372783690219520.0
2373883729018880.0
2373333709619200.0
2374433748418560.0
2373883729018880.0
2374983767818240.02374433748418560.0

2374983767818240.0
2375533787217920.0
2375533787217920.0
2376083806617600.0
2376083806617600.0
2376633826017280.0
2376633826017280.0
2377183845416960.0
2377183845416960.0
2377733864816640.0
2377733864816640.0
2378283884216320.0
2378283884216320.0
2378833903616000.0
2378833903616000.0
2379383923015680.02379383923015680.0

2379933942415360.0
2379933942415360.0
2380483961815040.0
2380483961815040.0
2381033981214720.0
2381033981214720.0
2381584000614400.0
2381584000614400.0
2382134020014080.0
2382134020014080.0
2382684039413760.0
2382684039413760.0
2383234058813440.0
2383234058813440.0
2383784078213120.0
2384334097612800.0
2383784078213120.0
2384884117012480.0
2384334097612800.0
2385434136412160.0
2384884117012480.0
2385984155811840.0
2385434136412160.02386534175211520.0

2385984155811840.0
2387084194611200.0
2387634214010880.0
2386534175211520.0
2388184233410560.0
2387084194611200.0
2388734252810240.02387634214010880.0

2388184233410560.0
2389284272209920.0
2388734252810240.0
2389834291609600.0
2389284272209920.0
2390384311009280.0
2389834291609600.0
2390934330408960.0
2390384311009280.0
2391484349808640.0
2390934330408960.0
2392034369208320.0
2391484349808640.0
2392034369208320.02392584388608000.0

2393134408007680.0
2392584388608000.0
2393684427407360.0
2393134408007680.0
2394234446807040.0
2393684427407360.0
2394784466206720.0
2394234446807040.0
2395334485606400.0
2394784466206720.0
2395884505006080.0
2395334485606400.0
2396434524405760.0
2395884505006080.0
2396984543805440.0
2396434524405760.0
2396984543805440.0
2397534563205120.0
2397534563205120.0
2398084582604800.02398084582604800.0

2398634602004480.0
2398634602004480.0
2399184621404160.0
2399184621404160.0
2399734640803840.0
2399734640803840.0
2400284660203520.0
2400284660203520.0
2400834679603200.0
2400834679603200.0
2401384699002880.0
2401384699002880.0
2401934718402560.0
2402484737802240.02401934718402560.0

2402484737802240.0
2403034757201920.0
2403034757201920.0
2403584776601600.0
2403584776601600.0
2404134796001280.0
2404134796001280.0
2404684815400960.0
2404684815400960.02405234834800640.0

2405234834800640.0
2405784854200320.0
2405784854200320.0
2406334873600000.0
2406884892999680.0
2406334873600000.0
2407434912399360.0
2406884892999680.0
2407984931799040.0
2407434912399360.0
2408534951198720.0
2407984931799040.0
2409084970598400.0
2408534951198720.0
2409634989998080.0
2409084970598400.0
2410185009397760.0
2410735028797440.02409634989998080.0

2411285048197120.02410185009397760.0

2410735028797440.0
2411835067596800.0
2411285048197120.0
2412385086996480.0
2411835067596800.0
2412935106396160.0
2412385086996480.0
2413485125795840.0
2412935106396160.0
2414035145195520.0
2413485125795840.0
2414585164595200.0
2414035145195520.0
2415135183994880.0
2414585164595200.0
2415135183994880.0
2415685203394560.0
2415685203394560.0
2416235222794240.0
2416235222794240.0
2416785242193920.0
2416785242193920.0
2417335261593600.0
2417335261593600.0
2417885280993280.0
2417885280993280.0
2418435300392960.0
2418435300392960.0
2418985319792640.0
2418985319792640.0
2419535339192320.0
2420085358592000.02419535339192320.0

2420085358592000.0
2420635377991680.0
2420635377991680.0
2421185397391360.0
2421185397391360.0
2421735416791040.0
2421735416791040.0
2422285436190720.0
2422285436190720.0
2422835455590400.0
2423385474990080.0
2422835455590400.0
2423935494389760.0
2423385474990080.0
2424485513789440.0
2423935494389760.0
2425035533189120.0
2424485513789440.0
2425585552588800.0
2425035533189120.0
2426135571988480.0
2425585552588800.0
2426685591388160.0
2426135571988480.0
2427235610787840.0
2426685591388160.0
2427785630187520.0
2427235610787840.0
2428335649587200.0
2427785630187520.0
2428885668986880.0
2428335649587200.0
2428885668986880.0
2429435688386560.0
2429435688386560.0
2429985707786240.0
2429985707786240.0
2430535727185920.0
2430535727185920.0
2431085746585600.0
2431085746585600.0
2431635765985280.0
2431635765985280.0
2432185785384960.0
2432185785384960.0
2432735804784640.0
2432735804784640.0
2433285824184320.0
2433285824184320.0
2433835843584000.0
2433835843584000.0
2434385862983680.0
2434385862983680.0
2434935882383360.0
2434935882383360.0
2435485901783040.0
2435485901783040.0
2436035921182720.0
2436035921182720.0
2436585940582400.0
2436585940582400.0
2437135959982080.0
2437135959982080.0
2437685979381760.0
2437685979381760.0
2438235998781440.0
2438235998781440.0
2438786018181120.0
2438786018181120.0
2439336037580800.0
2439336037580800.0
2439886056980480.0
2439886056980480.0
2440436076380160.0
2440436076380160.0
2440986095779840.0
2440986095779840.0
2441536115179520.0
2441536115179520.0
2442086134579200.0
2442086134579200.0
2442636153978880.0
2442636153978880.0
2443186173378560.0
2443186173378560.0
2443736192778240.0
2443736192778240.0
2444286212177920.0
2444286212177920.0
2444836231577600.0
2444836231577600.0
2445386250977280.0
2445386250977280.0
2445936270376960.0
2445936270376960.0
2446486289776640.0
2446486289776640.0
2447036309176320.0
2447036309176320.0
2447586328576000.0
2447586328576000.0
2448136347975680.0
2448136347975680.0
2448686367375360.0
2448686367375360.0
2449236386775040.0
2449236386775040.0
2449786406174720.0
2449786406174720.0
2450336425574400.0
2450336425574400.0
2450886444974080.0
2450886444974080.0
2451436464373760.0
2451436464373760.0
2451986483773440.0
2451986483773440.0
2452536503173120.0
2453086522572800.0
2452536503173120.0
2453636541972480.0
2453086522572800.0
2454186561372160.0
2453636541972480.0
2454736580771840.0
2454186561372160.0
2455286600171520.0
2454736580771840.0
2455836619571200.0
2455286600171520.0
2456386638970880.0
2455836619571200.0
2456936658370560.0
2456386638970880.0
2456936658370560.0
2457486677770240.0
2457486677770240.02458036697169920.0

2458586716569600.0
2458036697169920.0
2459136735969280.02458586716569600.0

2459686755368960.0
2459136735969280.0
2460236774768640.0
2459686755368960.0
2460786794168320.0
2460236774768640.0
2461336813568000.0
2460786794168320.0
2461886832967680.0
2461336813568000.0
2462436852367360.0
2461886832967680.0
2462986871767040.0
2462436852367360.0
2463536891166720.0
2462986871767040.0
2464086910566400.0
2463536891166720.0
2464636929966080.0
2465186949365760.0
2464086910566400.0
2465736968765440.0
2464636929966080.0
2466286988165120.0
2465186949365760.0
2466837007564800.0
2465736968765440.0
2467387026964480.0
2466286988165120.0
2467937046364160.0
2466837007564800.0
2468487065763840.0
2467387026964480.0
2469037085163520.0
2467937046364160.0
2469587104563200.0
2468487065763840.0
2469037085163520.0
2470137123962880.0
2469587104563200.0
2470687143362560.0
2470137123962880.0
2471237162762240.0
2470687143362560.0
2471787182161920.0
2471237162762240.0
2472337201561600.02471787182161920.0

2472337201561600.02472887220961280.0

2472887220961280.0
2473437240360960.0
2473437240360960.0
2473987259760640.0
2473987259760640.0
2474537279160320.0
2474537279160320.0
2475087298560000.0
2475087298560000.0
2475637317959680.0
2475637317959680.0
2476187337359360.0
2476187337359360.0
2476737356759040.0
2476737356759040.0
2477287376158720.0
2477287376158720.0
2477837395558400.0
2477837395558400.0
2478387414958080.0
2478387414958080.0
2478937434357760.0
2478937434357760.0
2479487453757440.0
2479487453757440.0
2480037473157120.0
2480037473157120.0
2480587492556800.0
2480587492556800.0
2481137511956480.0
2481137511956480.0
2481687531356160.0
2481687531356160.0
2482237550755840.0
2482237550755840.0
2482787570155520.0
2482787570155520.0
2483337589555200.0
2483337589555200.0
2483887608954880.0
2483887608954880.0
2484437628354560.0
2484437628354560.0
2484987647754240.02484987647754240.0

2485537667153920.0
2485537667153920.0
2486087686553600.0
2486087686553600.0
2486637705953280.0
2486637705953280.0
2487187725352960.0
2487187725352960.0
2487737744752640.0
2487737744752640.0
2488287764152320.02488287764152320.0

2488837783552000.02488837783552000.0

2489387802951680.0
2489387802951680.02489937822351360.0

2489937822351360.0
2490487841751040.0
2490487841751040.0
2491037861150720.0
2491037861150720.0
2491587880550400.0
2491587880550400.0
2492137899950080.0
2492137899950080.0
2492687919349760.0
2493237938749440.02492687919349760.0

2493787958149120.0
2493237938749440.0
2494337977548800.0
2493787958149120.0
2494887996948480.0
2494337977548800.0
2495438016348160.0
2494887996948480.0
2495988035747840.0
2495438016348160.0
2496538055147520.0
2495988035747840.0
2497088074547200.0
2496538055147520.0
2497638093946880.0
2497088074547200.0
2498188113346560.0
2497638093946880.0
2498738132746240.0
2498188113346560.0
2499288152145920.0
2498738132746240.0
2499838171545600.0
2499288152145920.0
2500388190945280.0
2499838171545600.0
2500938210344960.0
2500388190945280.0
2501488229744640.0
2500938210344960.0
2502038249144320.0
2501488229744640.0
2502588268544000.0
2502038249144320.0
2503138287943680.0
2502588268544000.0
2503688307343360.0
2503138287943680.0
2504238326743040.0
2503688307343360.0
2504788346142720.0
2504238326743040.0
2505338365542400.0
2504788346142720.0
2505888384942080.0
2505338365542400.0
2506438404341760.0
2505888384942080.0
2506988423741440.0
2506438404341760.0
2507538443141120.0
2506988423741440.0
2508088462540800.0
2507538443141120.0
2508638481940480.0
2508088462540800.0
2509188501340160.0
2508638481940480.0
2509738520739840.0
2509188501340160.0
2510288540139520.0
2509738520739840.0
2510838559539200.0
2510288540139520.0
2511388578938880.0
2510838559539200.0
2511938598338560.0
2511388578938880.0
2512488617738240.0
2511938598338560.0
2513038637137920.0
2512488617738240.0
2513588656537600.0
2513038637137920.0
2513588656537600.02514138675937280.0

2514688695336960.0
2514138675937280.0
2515238714736640.0
2514688695336960.0
2515788734136320.0
2515238714736640.0
2516338753536000.0
2515788734136320.0
2516888772935680.0
2516338753536000.0
2517438792335360.0
2516888772935680.0
2517988811735040.0
2517438792335360.0
2518538831134720.02517988811735040.0

2519088850534400.0
2518538831134720.0
2519638869934080.0
2519088850534400.0
2520188889333760.0
2519638869934080.0
2520738908733440.0
2520188889333760.0
2521288928133120.0
2520738908733440.0
2521838947532800.0
2521288928133120.0
2522388966932480.0
2521838947532800.0
2522938986332160.0
2522388966932480.0
2523489005731840.0
2522938986332160.0
2524039025131520.0
2524589044531200.0
2525139063930880.02523489005731840.0

2525689083330560.0
2524039025131520.0
2526239102730240.0
2524589044531200.0
2526789122129920.02525139063930880.0

2527339141529600.0
2525689083330560.0
2527889160929280.0
2526239102730240.0
2528439180328960.0
2526789122129920.0
2528989199728640.0
2527339141529600.0
2529539219128320.0
2527889160929280.0
2530089238528000.0
2528439180328960.0
2530639257927680.0
2528989199728640.0
2531189277327360.0
2529539219128320.0
2531739296727040.0
2530089238528000.0
2532289316126720.0
2530639257927680.0
2532839335526400.0
2531189277327360.0
2533389354926080.0
2531739296727040.0
2533939374325760.0
2532289316126720.0
2534489393725440.0
2532839335526400.0
2535039413125120.02533389354926080.0

2535589432524800.0
2533939374325760.0
2536139451924480.0
2534489393725440.0
2536689471324160.0
2535039413125120.0
2537239490723840.0
2535589432524800.0
2537789510123520.0
2536139451924480.0
2538339529523200.0
2536689471324160.0
2538889548922880.0
2537239490723840.0
2539439568322560.0
2537789510123520.0
2539989587722240.0
2538339529523200.0
2540539607121920.0
2538889548922880.02541089626521600.0

2541639645921280.0
2539439568322560.0
2542189665320960.0
2539989587722240.0
2542739684720640.0
2540539607121920.0
2541089626521600.02543289704120320.0

2543839723520000.0
2544389742919680.02541639645921280.0

2542189665320960.02544939762319360.0

2545489781719040.0
2542739684720640.0
2546039801118720.0
2543289704120320.0
2546589820518400.0
2543839723520000.0
2547139839918080.0
2544389742919680.0
2547689859317760.0
2544939762319360.0
2548239878717440.0
2548789898117120.0
2545489781719040.0
2549339917516800.0
2546039801118720.0
2549889936916480.0
2546589820518400.0
2547139839918080.0
2550439956316160.0
2547689859317760.0
2550989975715840.02548239878717440.0

2548789898117120.02551539995115520.0

2552090014515200.0
2549339917516800.0
2552640033914880.0
2549889936916480.0
2553190053314560.0
2550439956316160.0
2553740072714240.0
2550989975715840.0
2554290092113920.0
2551539995115520.0
2554840111513600.0
2552090014515200.0
2555390130913280.0
2552640033914880.0
2555940150312960.0
2553190053314560.0
2556490169712640.0
2553740072714240.0
2557040189112320.0
2554290092113920.0
2557590208512000.0
2554840111513600.0
2558140227911680.0
2555390130913280.0
2558690247311360.0
2555940150312960.0
2559240266711040.0
2556490169712640.0
2559790286110720.0
2557040189112320.0
2560340305510400.0
2557590208512000.0
2560890324910080.0
2558140227911680.0
2561440344309760.0
2561990363709440.02558690247311360.0

2559240266711040.0
2562540383109120.0
2559790286110720.0
2563090402508800.0
2560340305510400.0
2563640421908480.0
2560890324910080.0
2564190441308160.0
2561440344309760.0
2564740460707840.0
2561990363709440.0
2565290480107520.0
2562540383109120.0
2565840499507200.0
2563090402508800.0
2566390518906880.0
2563640421908480.0
2564190441308160.02566940538306560.0

2567490557706240.0
2564740460707840.0
2565290480107520.0
2568040577105920.0
2565840499507200.0
2568590596505600.0
2566390518906880.0
2569140615905280.0
2566940538306560.0
2569690635304960.0
2567490557706240.0
2570240654704640.0
2568040577105920.0
2568590596505600.02570790674104320.0

2571340693504000.0
2569140615905280.0
2571890712903680.0
2569690635304960.0
2572440732303360.0
2570240654704640.0
2572990751703040.0
2570790674104320.0
2573540771102720.0
2571340693504000.0
2574090790502400.0
2571890712903680.0
2572440732303360.02574640809902080.0

2575190829301760.0
2572990751703040.0
2575740848701440.0
2573540771102720.0
2576290868101120.0
2574090790502400.0
2576840887500800.0
2574640809902080.0
2577390906900480.0
2575190829301760.0
2577940926300160.0
2575740848701440.0
2578490945699840.0
2576290868101120.0
2579040965099520.0
2576840887500800.0
2579590984499200.02577390906900480.0

2580141003898880.0
2577940926300160.0
2580691023298560.0
2578490945699840.0
2581241042698240.0
2579040965099520.0
2581791062097920.0
2579590984499200.0
2582341081497600.0
2580141003898880.0
2582891100897280.0
2580691023298560.0
2583441120296960.0
2581241042698240.0
2583991139696640.0
2581791062097920.0
2584541159096320.0
2582341081497600.02585091178496000.0

2585641197895680.0
2582891100897280.0
2586191217295360.0
2583441120296960.0
2586741236695040.0
2583991139696640.0
2584541159096320.0
2587291256094720.0
2585091178496000.0
2587841275494400.0
2585641197895680.0
2588391294894080.0
2586191217295360.02588941314293760.0

2589491333693440.0
2586741236695040.0
2590041353093120.0
2587291256094720.0
2590591372492800.0
2587841275494400.0
2591141391892480.0
2588391294894080.0
2591691411292160.0
2588941314293760.0
2592241430691840.0
2589491333693440.0
2592791450091520.0
2590041353093120.0
2593341469491200.0
2590591372492800.0
2593891488890880.0
2591141391892480.0
2594441508290560.0
2591691411292160.0
2594991527690240.0
2592241430691840.0
2595541547089920.0
2592791450091520.0
2596091566489600.0
2593341469491200.0
2596641585889280.0
2593891488890880.0
2597191605288960.0
2594441508290560.0
2597741624688640.0
2594991527690240.0
2598291644088320.0
2598841663488000.0
2595541547089920.0
2599391682887680.0
2596091566489600.0
2599941702287360.0
2596641585889280.0
2600491721687040.0
2597191605288960.0
2601041741086720.0
2597741624688640.0
2601591760486400.0
2598291644088320.0
2602141779886080.0
2598841663488000.0
2602691799285760.0
2599391682887680.02603241818685440.0

2603791838085120.0
2599941702287360.0
2604341857484800.0
2600491721687040.0
2604891876884480.0
2601041741086720.0
2601591760486400.0
2605441896284160.0
2602141779886080.0
2605991915683840.0
2602691799285760.0
2606541935083520.0
2603241818685440.0
2607091954483200.0
2603791838085120.0
2607641973882880.0
2604341857484800.0
2608191993282560.0
2604891876884480.02608742012682240.0

2609292032081920.0
2605441896284160.0
2609842051481600.0
2605991915683840.0
2610392070881280.0
2606541935083520.0
2610942090280960.0
2607091954483200.0
2611492109680640.0
2607641973882880.0
2612042129080320.0
2608191993282560.0
2612592148480000.02608742012682240.0

2609292032081920.0
2613142167879680.0
2609842051481600.0
2613692187279360.0
2610392070881280.0
2614242206679040.0
2610942090280960.0
2614792226078720.0
2611492109680640.0
2615342245478400.0
2612042129080320.0
2615892264878080.0
2612592148480000.0
2616442284277760.0
2616992303677440.0
2613142167879680.0
2617542323077120.0
2613692187279360.0
2618092342476800.0
2614242206679040.0
2618642361876480.0
2614792226078720.0
2619192381276160.0
2615342245478400.0
2619742400675840.02615892264878080.0

2620292420075520.0
2616442284277760.0
2620842439475200.0
2616992303677440.0
2621392458874880.0
2617542323077120.0
2621942478274560.0
2618092342476800.0
2622492497674240.0
2618642361876480.0
2623042517073920.0
2619192381276160.0
2619742400675840.0
2623592536473600.0
2620292420075520.02624142555873280.0

2620842439475200.0
2624692575272960.0
2621392458874880.0
2625242594672640.0
2621942478274560.0
2625792614072320.0
2622492497674240.0
2626342633472000.0
2623042517073920.0
2626892652871680.0
2623592536473600.0
2627442672271360.0
2624142555873280.0
2627992691671040.0
2624692575272960.0
2628542711070720.0
2625242594672640.0
2629092730470400.0
2625792614072320.0
2629642749870080.0
2626342633472000.0
2630192769269760.0
2626892652871680.0
2630742788669440.0
2627442672271360.0
2631292808069120.0
2627992691671040.0
2631842827468800.0
2628542711070720.0
2632392846868480.0
2629092730470400.0
2629642749870080.02632942866268160.0

2633492885667840.0
2630192769269760.0
2634042905067520.0
2630742788669440.0
2634592924467200.0
2631292808069120.0
2631842827468800.0
2632392846868480.0
2635142943866880.0
2632942866268160.02635692963266560.0

2636242982666240.0
2633492885667840.0
2636793002065920.0
2634042905067520.0
2637343021465600.0
2634592924467200.0
2637893040865280.0
2635142943866880.0
2638443060264960.0
2635692963266560.0
2638993079664640.0
2636242982666240.0
2639543099064320.0
2636793002065920.0
2637343021465600.0
2640093118464000.0
2637893040865280.0
2640643137863680.0
2638443060264960.02641193157263360.0

2641743176663040.0
2638993079664640.0
2642293196062720.0
2639543099064320.0
2642843215462400.0
2640093118464000.0
2643393234862080.0
2640643137863680.0
2643943254261760.0
2641193157263360.0
2644493273661440.0
2641743176663040.0
2645043293061120.0
2642293196062720.0
2645593312460800.0
2642843215462400.0
2646143331860480.0
2643393234862080.0
2646693351260160.0
2643943254261760.0
2647243370659840.0
2644493273661440.0
2647793390059520.0
2645043293061120.0
2648343409459200.0
2645593312460800.0
2648893428858880.0
2646143331860480.0
2649443448258560.0
2646693351260160.0
2649993467658240.0
2647243370659840.0
2650543487057920.0
2647793390059520.0
2651093506457600.0
2648343409459200.0
2651643525857280.0
2652193545256960.0
2648893428858880.0
2652743564656640.0
2649443448258560.0
2653293584056320.0
2649993467658240.0
2653843603456000.0
2650543487057920.0
2654393622855680.0
2651093506457600.0
2654943642255360.0
2651643525857280.0
2655493661655040.0
2652193545256960.0
2656043681054720.0
2652743564656640.0
2656593700454400.0
2653293584056320.0
2657143719854080.0
2653843603456000.0
2654393622855680.0
2657693739253760.0
2654943642255360.0
2658243758653440.0
2655493661655040.0
2658793778053120.0
2656043681054720.0
2659343797452800.0
2656593700454400.0
2659893816852480.02657143719854080.0

2657693739253760.0
2660443836252160.0
2658243758653440.0
2660993855651840.0
2658793778053120.0
2661543875051520.0
2659343797452800.0
2662093894451200.0
2659893816852480.0
2662643913850880.0
2660443836252160.0
2663193933250560.0
2660993855651840.0
2663743952650240.0
2661543875051520.0
2664293972049920.0
2662093894451200.0
2664843991449600.0
2662643913850880.0
2665394010849280.0
2663193933250560.0
2665944030248960.0
2663743952650240.02666494049648640.0

2664293972049920.0
2667044069048320.0
2664843991449600.0
2667594088448000.0
2665394010849280.0
2668144107847680.0
2665944030248960.0
2668694127247360.0
2666494049648640.0
2669244146647040.0
2667044069048320.0
2669794166046720.0
2667594088448000.0
2670344185446400.0
2668144107847680.0
2670894204846080.0
2668694127247360.0
2671444224245760.0
2669244146647040.0
2671994243645440.0
2669794166046720.02672544263045120.0

2673094282444800.0
2670344185446400.0
2673644301844480.0
2670894204846080.0
2674194321244160.0
2671444224245760.0
2674744340643840.0
2671994243645440.0
2675294360043520.0
2672544263045120.0
2675844379443200.0
2673094282444800.0
2676394398842880.0
2673644301844480.0
2676944418242560.0
2674194321244160.0
2677494437642240.0
2674744340643840.0
2678044457041920.0
2675294360043520.0
2678594476441600.0
2675844379443200.0
2679144495841280.0
2676394398842880.0
2676944418242560.02679694515240960.0

2680244534640640.0
2677494437642240.0
2680794554040320.0
2678044457041920.0
2681344573440000.0
2678594476441600.0
2681894592839680.0
2679144495841280.0
2682444612239360.0
2679694515240960.0
2682994631639040.0
2680244534640640.0
2683544651038720.0
2680794554040320.0
2684094670438400.0
2681344573440000.0
2684644689838080.0
2681894592839680.0
2685194709237760.0
2682444612239360.0
2685744728637440.0
2682994631639040.0
2686294748037120.0
2683544651038720.0
2686844767436800.0
2684094670438400.0
2687394786836480.0
2684644689838080.0
2687944806236160.0
2685194709237760.0
2688494825635840.0
2685744728637440.02689044845035520.0

2689594864435200.0
2686294748037120.0
2690144883834880.0
2686844767436800.0
2690694903234560.0
2687394786836480.0
2691244922634240.0
2687944806236160.0
2691794942033920.0
2688494825635840.0
2692344961433600.0
2689044845035520.0
2692894980833280.0
2689594864435200.0
2693445000232960.0
2690144883834880.0
2693995019632640.0
2690694903234560.0
2694545039032320.0
2691244922634240.0
2695095058432000.0
2691794942033920.0
2695645077831680.0
2692344961433600.0
2696195097231360.0
2692894980833280.0
2696745116631040.0
2693445000232960.0
2697295136030720.0
2693995019632640.0
2697845155430400.0
2694545039032320.0
2698395174830080.0
2695095058432000.0
2698945194229760.0
2695645077831680.02699495213629440.0

2700045233029120.0
2696195097231360.0
2700595252428800.0
2696745116631040.0
2701145271828480.02697295136030720.0

2697845155430400.0
2701695291228160.0
2698395174830080.0
2702245310627840.0
2698945194229760.0
2702795330027520.0
2699495213629440.0
2703345349427200.0
2700045233029120.0
2703895368826880.0
2700595252428800.0
2704445388226560.0
2701145271828480.0
2704995407626240.0
2701695291228160.0
2705545427025920.0
2702245310627840.0
2706095446425600.0
2702795330027520.0
2706645465825280.0
2703345349427200.0
2707195485224960.0
2707745504624640.0
2703895368826880.0
2708295524024320.0
2704445388226560.0
2708845543424000.0
2704995407626240.0
2709395562823680.0
2705545427025920.0
2709945582223360.0
2706095446425600.0
2710495601623040.0
2706645465825280.0
2711045621022720.0
2707195485224960.0
2711595640422400.0
2707745504624640.0
2708295524024320.0
2712145659822080.0
2708845543424000.02712695679221760.0

2713245698621440.0
2709395562823680.0
2713795718021120.0
2709945582223360.0
2714345737420800.0
2710495601623040.0
2714895756820480.0
2711045621022720.0
2715445776220160.0
2711595640422400.0
2715995795619840.0
2712145659822080.0
2716545815019520.0
2712695679221760.0
2717095834419200.0
2717645853818880.0
2713245698621440.0
2718195873218560.0
2713795718021120.0
2718745892618240.0
2714345737420800.0
2719295912017920.0
2714895756820480.0
2719845931417600.0
2715445776220160.0
2720395950817280.0
2715995795619840.0
2720945970216960.0
2716545815019520.0
2721495989616640.0
2717095834419200.0
2722046009016320.02717645853818880.0

2718195873218560.0
2722596028416000.0
2718745892618240.0
2723146047815680.0
2719295912017920.0
2723696067215360.0
2719845931417600.0
2724246086615040.0
2720395950817280.0
2724796106014720.0
2720945970216960.0
2725346125414400.0
2721495989616640.0
2725896144814080.0
2722046009016320.0
2722596028416000.0
2726446164213760.0
2723146047815680.0
2726996183613440.0
2723696067215360.0
2727546203013120.0
2724246086615040.0
2728096222412800.0
2724796106014720.0
2728646241812480.0
2725346125414400.0
2729196261212160.0
2725896144814080.0
2729746280611840.0
2726446164213760.0
2730296300011520.02726996183613440.0

2727546203013120.0
2730846319411200.0
2731396338810880.0
2728096222412800.0
2731946358210560.0
2728646241812480.0
2732496377610240.0
2729196261212160.0
2733046397009920.0
2729746280611840.0
2733596416409600.0
2730296300011520.0
2734146435809280.0
2730846319411200.0
2734696455208960.0
2731396338810880.0
2735246474608640.0
2731946358210560.0
2735796494008320.0
2732496377610240.0
2736346513408000.0
2733046397009920.0
2736896532807680.0
2733596416409600.0
2737446552207360.0
2734146435809280.0
2737996571607040.0
2734696455208960.0
2738546591006720.0
2735246474608640.0
2739096610406400.0
2735796494008320.0
2739646629806080.0
2736346513408000.0
2740196649205760.0
2736896532807680.0
2740746668605440.0
2737446552207360.0
2741296688005120.0
2737996571607040.0
2741846707404800.0
2738546591006720.0
2742396726804480.0
2739096610406400.0
2742946746204160.0
2739646629806080.0
2743496765603840.0
2744046785003520.0
2740196649205760.0
2740746668605440.0
2744596804403200.0
2741296688005120.0
2745146823802880.0
2741846707404800.02745696843202560.0

2746246862602240.0
2742396726804480.0
2746796882001920.0
2742946746204160.0
2747346901401600.0
2743496765603840.0
2747896920801280.0
2744046785003520.0
2748446940200960.0
2744596804403200.0
2748996959600640.0
2745146823802880.0
2749546979000320.0
2745696843202560.0
2750096998400000.0
2746246862602240.0
2750647017799680.0
2746796882001920.0
2751197037199360.02747346901401600.0

2747896920801280.0
2751747056599040.0
2748446940200960.0
2752297075998720.0
2748996959600640.02752847095398400.0

2753397114798080.0
2749546979000320.0
2753947134197760.0
2750096998400000.0
2754497153597440.0
2750647017799680.0
2751197037199360.0
2751747056599040.0
2752297075998720.0
2752847095398400.0
2753397114798080.0
2753947134197760.0

```

#### GPU

Heavily used in Deep Learning

*   NVidia
  - Numba [CUDA GPU ](http://numba.pydata.org/numba-doc/latest/cuda/index.html)
*  AMD
  - Numba [AMD ROC GPU](http://numba.pydata.org/numba-doc/latest/roc/index.html)



##### Use GPU



{:.input_area}
```
@vectorize(['float32(float32, float32)'], target='cuda')
def add_ufunc(x, y):
    return x + y
  
def cuda_operation():
    """Performs Vectorized Operations on GPU"""

    x = real_estate_array()
    y = real_estate_array()

    print("Moving calculations to GPU memory")
    x_device = cuda.to_device(x)
    y_device = cuda.to_device(y)
    out_device = cuda.device_array(
        shape=(x_device.shape[0],x_device.shape[1]), dtype=np.float32)
    print(x_device)
    print(x_device.shape)
    print(x_device.dtype)

    print("Calculating on GPU")
    add_ufunc(x_device,y_device, out=out_device)

    out_host = out_device.copy_to_host()
    print(f"Calculations from GPU {out_host}")
    
cuda_operation()
```


#### TPU

[Tensor Processing Unit](https://cloud.google.com/tpu/docs/tpus)



*   "Google’s custom-developed application-specific integrated circuits (ASICs) used to accelerate machine learning workloads"
*   Available both in colab notebooks and on Google Cloud


