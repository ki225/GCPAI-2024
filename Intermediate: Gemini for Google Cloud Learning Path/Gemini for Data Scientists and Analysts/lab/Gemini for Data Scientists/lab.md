# Gemini for Data Scientists

## Objectives
In this lab, you learn how to perform the following tasks:

- Use Colab Enterprise Python notebooks inside BigQuery Studio.
- Use BigQuery DataFrames inside of BigQuery Studio.
- Use Gemini to generate code from natural language prompts.
- Build a K-means clustering model.
- Generate a visualization of the clusters.
- Use the text-bison model to develop the next steps for a marketing campaign.
- Clean up project resources.


## Task
### Task 1. Create a BigQuery dataset for your project



### Task 2. Create a new Python notebook


### Task 3. Connect to the Colab Enterprise runtime in BigQuery


### Task 4. Build the Python Notebook
#### Import Python libraries and define variables
```
from google.cloud import bigquery
from google.cloud import aiplatform
import bigframes.pandas as bpd
import pandas as pd
from vertexai.language_models._language_models import TextGenerationModel
from bigframes.ml.cluster import KMeans
from bigframes.ml.model_selection import train_test_split
```

| **Library**              | **Description**                                                                 |
|--------------------------|---------------------------------------------------------------------------------|
| BigQuery                 | Python Client for Google BigQuery                                                |
| AI Platform              | Vertex AI SDK for Python                                                         |
| bigframes.pandas         | BigQuery DataFrames                                                              |
| pandas                   | Open source data analysis and manipulation tool, built on top of Python          |
| TextGenerationModel      | Creates a LanguageModel within Vertex AI                                         |
| Kmeans                   | Used to create K-means clustering models within BigQuery DataFrames              |
| train_test_split         | Used to split source dataset into test and training subsets, used with model tuning within BigQuery DataFrames |



#### Define variables and initiate the BigQuery and Vertex AI connection
```
project_id = '<project_id>'
dataset_name = "ecommerce"
model_name = "customer_segmentation_model"
table_name = "customer_stats"
location = "<location>"
client = bigquery.Client(project=project_id)
aiplatform.init(project=project_id, location=location)
``` 

#### Create and import the ecommerce.customer_stats table
create a K-means clustering model to split the customer data into clusters based on fields like order recency, order count, and spend, and you will then visualize these as groups within a chart directly within the notebook.
```
# prompt: Convert the table ecommerce.customer_stats to a bigframes dataframe and show the top 10 records

customer_df = bpd.read_gbq(f'{project_id}.{dataset_name}.{table_name}')
customer_df.head(10)
```

#### Generate the K-means clustering model
```
# prompt: 1. Split df (using random state and test size 0.2) into test and training data for a K-means clustering algorithm store these as df_test and df_train. 2. Create a K-means cluster model using bigframes.ml.cluster KMeans with 5 clusters. 3. Save the model using the to_gbq method where the model name is project_id.dataset_name.model_name.

df_train, df_test = train_test_split(customer_df, test_size=0.2, random_state=42)
kmeans = KMeans(n_clusters=5)
kmeans.fit(df_train)
kmeans.to_gbq(f'{project_id}.{dataset_name}.{model_name}')
```
define a new BigQuery DataFrame that joins the segment/cluster produced by the K-means model back to the original data.
```
# prompt: 1. Call the K-means prediction model on the df dataframe, and store the results as predictions_df and show the first 10 records.

predictions_df = kmeans.predict(df_test)
predictions_df.head(10)
```

#### Create a visualization of the K-means clustering model results
create a visualization of the K-means clustering model results. Specifically, you will generate a scatterplot using predictions_df to look at the relationship between the days since last order by average spend, colored by segment_id (which was generated using our K-means model!)

```
# prompt: 1. Using predictions_df, and matplotlib, generate a scatterplot. 2. On the x-axis of the scatterplot, display days_since_last_order and on the y-axis, display average_spend from predictions_df. 3. Color by cluster. 4. The chart should be titled "Attribute grouped by K-means cluster."

import matplotlib.pyplot as plt

# Create the scatter plot
plt.figure(figsize=(10, 6))
for cluster_id in predictions_df['CENTROID_ID'].unique():
  cluster_data = predictions_df[predictions_df['CENTROID_ID'] == cluster_id]
  plt.scatter(cluster_data['days_since_last_order'], cluster_data['average_spend'], label=f'Cluster {cluster_id}')

# Customize the plot
plt.title('Attribute grouped by K-means cluster')
plt.xlabel('Days Since Last Order')
plt.ylabel('Average Spend')
plt.legend(loc='best')
plt.show()
```


### Task 5. Generate insights from the results of the model
#### Summarize each cluster generated from the K-means model
summarize each cluster generated from the K-means model.
```
query = """
SELECT
 CONCAT('cluster ', CAST(centroid_id as STRING)) as centroid,
 average_spend,
 count_orders,
 days_since_last_order
FROM (
 SELECT centroid_id, feature, ROUND(numerical_value, 2) as value
 FROM ML.CENTROIDS(MODEL `{0}.{1}`)
)
PIVOT (
 SUM(value)
 FOR feature IN ('average_spend',  'count_orders', 'days_since_last_order')
)
ORDER BY centroid_id
""".format(dataset_name, model_name)

df_centroid = client.query(query).to_dataframe()
df_centroid.head()
```

convert the data frame into a string, so you can pass it to your large language model call.

```
df_query = client.query(query).to_dataframe()
df_query.to_string(header=False, index=False)

cluster_info = []
for i, row in df_query.iterrows():
 cluster_info.append("{0}, average spend ${2}, count of orders per person {1}, days since last order {3}"
  .format(row["centroid"], row["count_orders"], row["average_spend"], row["days_since_last_order"]) )

cluster_info = (str.join("\n", cluster_info))
print(cluster_info)
```

#### Define a prompt for the marketing campaign
```
prompt = f"""
You're a creative brand strategist, given the following clusters, come up with \
creative brand persona, a catchy title, and next marketing action, \
explained step by step.

Clusters:
{cluster_info}

For each Cluster:
* Title:
* Persona:
* Next marketing step:
"""
```

#### Generate the marketing campaign using the text-bison model
call the text-bison model to generate customer insights and next steps for our marketing team.

For each cluster/segment defined by the K-means model, we'll generate three items that can be used by our marketing team:

- Title
- Persona
- Next marketing step

```
# prompt: Use the Vertex AI language_models API to call the PaLM2 text-bison model and generate a marketing campaign using the variable prompt. Use the following model settings: max_output_tokens=1024, temperature=0.4

model = TextGenerationModel.from_pretrained("text-bison@001")
response = model.predict(
    prompt,
    max_output_tokens=1024,
    temperature=0.4,
)
print(response.text)
```

### Task 6. Clean up project resources (Optional)
#### Clean up resources by removing the project
1. In the Google Cloud console, go to the IAM & Admin > Manage resources page.
2. In the project list, select the project that you want to delete, and then click Delete.
3. In the dialog, type the project ID, and then click Shut Down Anyway to delete the project.


#### Clean up resources by deleting individual resources
```
# Delete customer_stats table

client.delete_table(f"{project_id}.{dataset_name}.{table_name}", not_found_ok=True)
print(f"Deleted table: {project_id}.{dataset_name}.{table_name}")


# Delete K-means model
client.delete_model(f"{project_id}.{dataset_name}.{model_name}", not_found_ok=True)
print(f"Deleted model: {project_id}.{dataset_name}.{model_name}")
```







