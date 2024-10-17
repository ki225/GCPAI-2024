# Develop an app with Gemini

## Objectives
In this lab, you learn how to perform the following tasks:

1. Create a cloud-based application development environment by using Cloud Workstations.
2. Explore various Google services that you can use to deploy an app by asking Gemini context-based questions.
3. Prompt Gemini to provide templates that you can use to develop a basic app in Cloud Run.
4. Create, explore, and modify the app by using Gemini to explain and generate the code.
5. Run and test the app locally, and then deploy it to Google Cloud by using Gemini to generate the steps.

## Task
### Task 1. Configure your environment and account
To set your project ID and region environment variables, in Cloud Shell, run the following commands:
```
PROJECT_ID=$(gcloud config get-value project)
REGION=us-east4
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
### Task 2. Create a Cloud Workstation


#### Create a workstation configuration


#### Launch the IDE


### Task 3. Update the Cloud Code extension to enable Gemini


### Task 4. Chat with Gemini


### Task 5. Develop a Python app


### Task 6. Enhance the Python app

### Task 7. Deploy the app to Cloud Run











