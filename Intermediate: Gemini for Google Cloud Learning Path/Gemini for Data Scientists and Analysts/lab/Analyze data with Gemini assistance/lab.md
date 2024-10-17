# Analyze data with Gemini assistance

## Objectives
In this lab, you learn how to perform the following tasks:

- Use Gemini to answer your questions about Google Cloud data analytics products and use cases.
- Prompt Gemini to explain and generate SQL queries in BigQuery.
- Build a machine learning (ML) model to forecast future periods.

## Tasks
### Task 1. Configure your environment and account
To set your project ID and region environment variables, in Cloud Shell, run the following commands:

```
PROJECT_ID=$(gcloud config get-value project)
REGION=us-central1
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```
To store the signed-in Google user account in an environment variable, run the following command:
```
USER=$(gcloud config get-value account 2> /dev/null)
echo "USER=${USER}"
```
Enable the Cloud AI Companion API for Gemini:
```
gcloud services enable cloudaicompanion.googleapis.com --project ${PROJECT_ID}
```
To use Gemini, grant the necessary IAM roles to your Google Cloud Qwiklabs user account:
```
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/cloudaicompanion.user
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/serviceusage.serviceUsageViewer
```

### Task 2. Create a dataset and enable Gemini features in BigQuery
#### Create a dataset
In the Google Cloud console, select the Navigation menu (Navigation menu icon), and then select BigQuery. If prompted click Done.


#### Enable the Gemini features in BigQuery
To view Gemini features in BigQuery, in the toolbar, click Gemini (Gemini). If it's not visible, refresh the page.

### Task 3. Use Gemini to analyze your data
#### Prompt Gemini to answer questions about your data
1. In the Google Cloud console, select the Navigation menu (Navigation menu icon), and then select BigQuery.
2. In the Google Cloud console toolbar, click Open Gemini (console Gemini menu).
3. Click ENABLE to enable the Gemini for Google Cloud API.
4. The Welcome to Gemini in Cloud Console message is displayed in the Gemini pane. Click Start Chatting.


Q:
```
How do I learn which datasets and tables are available to me in BigQuery?
```
A:
```
You can discover the datasets and tables available to you in BigQuery using several methods:

1. BigQuery UI:

Navigate to the BigQuery UI: Log in to your Google Cloud Console and go to the BigQuery section.
Explore Datasets: In the left-hand navigation panel, you'll see a list of your projects. Expand a project to view its datasets.
View Tables: Click on a dataset to see the tables within it.
2. bq ls Command-Line Tool:

Open a terminal or command prompt: Make sure you have the bq command-line tool installed.
List Datasets: Use the command bq ls to list all datasets in your project.
List Tables: Use the command bq ls [dataset_id] to list all tables within a specific dataset.
3. INFORMATION_SCHEMA :

Query the INFORMATION_SCHEMA : This built-in schema provides metadata about your BigQuery environment.
List Datasets: Use a query like SELECT DISTINCT table_schema FROM bigquery-public-data.baseball.INFORMATION_SCHEMA.COLUMNS to list all datasets.
List Tables: Use a query like SELECT DISTINCT table_name FROM bigquery-public-data.baseball.INFORMATION_SCHEMA.COLUMNS to list all tables within a dataset.
4. BigQuery API:

Use the BigQuery API: You can programmatically access BigQuery using the API.
List Datasets: Use the tables.list method to list datasets.
List Tables: Use the tables.list method with a dataset ID to list tables within a dataset.
5. BigQuery Public Datasets:

Explore Public Datasets: BigQuery offers a collection of public datasets that you can access and explore.
Find Datasets: Go to the BigQuery UI and navigate to the "Public Datasets" section.
Browse Tables: Click on a public dataset to see the available tables.
Remember that the datasets and tables you have access to depend on your project's permissions and the data sharing settings.
```
### Task 4. Prompt Gemini to explain SQL queries in a sales dataset
```
SELECT u.id as user_id, u.first_name, u.last_name, avg(oi.sale_price) as avg_sale_price   
FROM `bigquery-public-data.thelook_ecommerce.users` as u   
JOIN `bigquery-public-data.thelook_ecommerce.order_items` as oi   
ON u.id = oi.user_id   
GROUP BY 1,2,3   
ORDER BY avg_sale_price DESC   
LIMIT 10
```


### Task 5. Generate a SQL query that groups sales by day and product
#### Review the order_items and products tables the public dataset


#### Use a prompt to generate the query
prompt
```
 # select the sum of sales by date, and product_id casted to day from order_items, joined with products. Include the product name in the results. Round the total_sales field to two decimal places and order results by total_sales descending.
```
```
 # select the sum of sales by date, and product_id casted to day from order_items, joined with products. Include the product name in the results. Round the total_sales field to two decimal places and order results by total_sales descending.
 SELECT
     DATE(order_items.created_at) AS order_date,
     order_items.product_id,
     products.name AS product_name,
     ROUND(SUM(order_items.sale_price), 2) AS total_sales
 FROM
     `bigquery-public-data.thelook_ecommerce.order_items` AS order_items
 LEFT JOIN
     `bigquery-public-data.thelook_ecommerce.products` AS products
 ON
     order_items.product_id = products.id
 GROUP BY
     order_date,
     order_items.product_id,
     product_name
 ORDER BY
     total_sales DESC;
```

### Task 6. Build a forecasting model and view results

#### Build the model
You use the following example query with actual sales, which are used as an input to the model. The query is used as a part of creating the ML model.
```
CREATE MODEL bqml_tutorial.sales_forecasting_model
OPTIONS(MODEL_TYPE='ARIMA_PLUS',
time_series_timestamp_col='date_col',
time_series_data_col='total_sales',
time_series_id_col='product_id') AS
SELECT sum(sale_price) as total_sales,
DATE(created_at) as date_col,
product_id
FROM `bigquery-public-data.thelook_ecommerce.order_items`
AS t1
INNER JOIN `bigquery-public-data.thelook_ecommerce.products`
AS t2
ON t1.product_id = t2.id
GROUP BY 2, 3;
```



#### Query the model with a prompt

```
# Use sales_forecasting_model from the bqml_tutorial dataset in my project to generate a forecast and return all the resulting data.
SELECT
    *
  FROM
    ML.FORECAST(MODEL `bqml_tutorial.sales_forecasting_model`, STRUCT(10 AS horizon));
```




