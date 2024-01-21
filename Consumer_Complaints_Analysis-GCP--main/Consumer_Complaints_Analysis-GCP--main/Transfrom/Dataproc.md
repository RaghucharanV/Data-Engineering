# TRANSFORM

**Transformation of data by using Dataproc**

**DATAPROC**

  Dataproc is a managed Spark and Hadoop service that lets you take advantage of open source data tools for batch processing, querying, streaming, and machine learning.
  We can build use spark,python,Hadoop, to transfrom data.

# Prerequisite
  1.Create a Dataproc api in Project("Text-Mining"), based on requirements

  2.Open Web Interfaces, bottom Click on Jupiter notebook

  3.Create a New Notebook, based on your requirements(here,python3 kernal created).


# Processs

1. Extracted data from Cloud using google.cloud library


```python
import pandas as pd
from google.cloud import storage

# Cloud Storage etails
bucket_name = 'customer_compliant'
file_path = 'Consumer_Complaints.csv'

# Local file path to download data
local_file_path = 'Customercomplaints.csv'

# Authenticate to Google Cloud Storage
storage_client = storage.Client()

# Download the file from Cloud Storage
bucket = storage_client.get_bucket(bucket_name)
blob = bucket.blob(file_path)
blob.download_to_filename(local_file_path)

df = pd.read_csv(local_file_path, low_memory=False)
```



   
2. Performed Text Data Analysis

```python
df.shape, df.size
#((670598, 18), 12070764)
#Droping unnecessary columns
df1 = df1.drop(['Sub-issue','Consumer complaint narrative','Company public response','Consumer consent provided?'], axis=1)
#Finding Unique values in categorical Columns
df1['Product'].value_counts()

```



3. Most of Data Contain "Nan" values. So Handing missing data is important for features Extraction


```python

#filling missing values with most repeated values in column.
df1['State']= df1['State'].fillna('CA')
#Droping missing values
df1 = df1.dropna(subset='Consumer disputed?')
df1 = df1.dropna(subset=['ZIP code'])

```


4. Changing Date-time from object type


```python

df1['Date sent to company'] = pd.to_datetime(df1['Date sent to company'])
df1['Date received'] = pd.to_datetime(df1['Date received'])

```




5. Saving file to Cloud to Load in Big-Query


```python

df1.to_csv("Consumer_Complaints_transformed.csv")
processed_file_path = 'Consumer_Complaints_transformed.csv'
blob = bucket.blob(processed_file_path)
blob.upload_from_filename(processed_file_path)
```



By this you can transform code by using pyspark based on requirement....
