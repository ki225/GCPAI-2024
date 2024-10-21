# Deploy a Streamlit App Integrated with Gemini Pro on Cloud Run

## Tasks
### Task 1. Run the Application Locally
Run the following commands to clone the repo and navigate to gemini-streamlit-cloudrun directory in Cloud Shell using the following commands.
```
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
```
#### Run the application
1. Setup the Python virtual environment and install the dependencies:
```
python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate
pip install -r requirements.txt
```
2. Your application requires access to two environment variables:
   - GCP_PROJECT : This the Google Cloud project ID.
   - GCP_REGION : This is the region in which you are deploying your Cloud Run app. For e.g. us-central1.
```
GCP_PROJECT='qwiklabs-gcp-01-f96e09b135a6'
GCP_REGION='us-east4'
```
3. To run the application locally, execute the following command.
```
streamlit run app.py \
  --browser.serverAddress=localhost \
  --server.enableCORS=false \
  --server.enableXsrfProtection=false \
  --server.port 8080
```
4. The application will startup and you will be provided a URL to the application. Click the link to view the application in the browser or use Cloud Shell's web preview function to launch the preview page.
5. Adjust the parameters for the story generation and click Generate my story.
6. Navigate back to Cloud Shell and authorize the application to access the Gemini API. Once you have authorized the application, you can navigate back to the application to see the response.


### Task 2. Build and Deploy the Application to Cloud Run
You will now build the Docker image for the application and push it to Artifact Registry. To do this, you will need one environment variable set that will point to the Artifact Registry name. The commands below will create this Artifact Registry repository for you.

In Cloud Shell, execute the following command:

```
AR_REPO='gemini-repo'
SERVICE_NAME='gemini-streamlit-app' 
gcloud artifacts repositories create "$AR_REPO" --location="$GCP_REGION" --repository-format=Docker
gcloud builds submit --tag "$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME"
```

The final step is to deploy the service in Cloud Run with the image that we had built and had pushed to the Artifact Registry in the previous step.
```
gcloud run deploy "$SERVICE_NAME" \
  --port=8080 \
  --image="$GCP_REGION-docker.pkg.dev/$GCP_PROJECT/$AR_REPO/$SERVICE_NAME" \
  --allow-unauthenticated \
  --region=$GCP_REGION \
  --platform=managed  \
  --project=$GCP_PROJECT \
  --set-env-vars=GCP_PROJECT=$GCP_PROJECT,GCP_REGION=$GCP_REGION
```


