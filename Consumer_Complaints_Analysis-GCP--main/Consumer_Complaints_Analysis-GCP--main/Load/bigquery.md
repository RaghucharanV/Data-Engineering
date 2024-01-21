# LOAD

**Loading Data into Big-Query***

**BIG-QUERY**

  BigQuery is a fully-managed, serverless, and cost-effective data warehouse in Google Cloud Platform (GCP). 
  It's a database-as-a-service (DBaaS) that supports the querying and analysis of enterprise data.BigQuery is better at storing and handling large amounts of data than Knime. 
  BigQuery is SQL (Structured Query Language) powered, which structures data into tables, rows, and columns.
  It supports fast SQL queries using the processing power of Google's infrastructure. BigQuery also has built-in features like machine learning, geospatial analysis, and business intelligence. 

# Prerequisite

  1. Create a Big-query Api on your project ("Text-mining")
  2. Connect to Cloud Storage

**Process**
1. Load the Transformed Data file from the cloud to Bigquery by connecting to the cloud and loading it. It also builds the schema and Table while uploading Cloud file.

![Screenshot (823)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/dd3aefb3-f827-4006-866b-ac7313c71ff3)

![Screenshot (824)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/ab483030-3379-401d-941c-eaf774795271)

2. Quering the big data table, emphasizing the performance.
![Screenshot (834)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/ab84952f-feef-4e53-98e8-6f9c91954b6e)


```SQL

SELECT * FROM `text-mining-407810.complaintscus.complaints_table` LIMIT 1000 
```

![Screenshot (836)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/89c8f23e-2dfd-4a52-be6f-ff1df761fe38)



```SQL

SELECT Response_time FROM `text-mining-407810.complaintscus.complaints_table` WHERE Response_time>0 LIMIT 1000 
```

![Screenshot (838)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/893ef534-611b-45af-be49-1a463976b314)


```SQL

SELECT * FROM `text-mining-407810.complaintscus.complaints_table` WHERE Company LIKE 'JPMorgan Chase & Co.' LIMIT 1000
```


![Screenshot (837)](https://github.com/RaghucharanV/Consumer_Complaints_Analysis-GCP-/assets/81848656/7641c6b0-55e8-419c-95a2-20fc8ce5098c)


3. Altering the Table Unnamed and dropping from the Table.


You can build and query any size of data the performance in Bigquery you see is Un-predictable....
