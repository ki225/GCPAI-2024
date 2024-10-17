# Provision Cloud Infrastructure with Gemini

## Objectives
In this lab, you will learn how to perform the following tasks:

- Enable Gemini
- Explore various Google services that you can use to deploy an app to GKE by asking Gemini context-based questions.
- Prompt Gemini to provide commands that you can use to deploy a basic app to a GKE cluster.
- Create, explore, and modify the GKE cluster by using Gemini to explain and generate the shell commands.

## Tasks
### Task 1. Enable Gemini
To set your project ID and region environment variables, run the following commands:
```
PROJECT_ID=$(gcloud config get-value project)
REGION=us-west1
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```

> [!Note]
> It is worth noting the region, as it may differ to the sample prompts below.
To store the signed-in Google user account in an environment variable, run the following command:

```
USER=$(gcloud config get-value account 2> /dev/null)
echo "USER=${USER}"
```

Click Authorize if prompted.

Enable the Cloud AI Companion API for Gemini:
```
gcloud services enable cloudaicompanion.googleapis.com --project ${PROJECT_ID}
```
To use Gemini, grant the necessary IAM roles to your Google Cloud Qwiklabs user account:
```
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/cloudaicompanion.user
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/serviceusage.serviceUsageViewer
```

Adding these roles lets the user use Gemini assistance.

### Task 2. Deploy GKE clusters

> Gemini doesn't use your prompts or its responses as data to train its model. For more information, see How Gemini in Google Cloud uses your data.


### Task 3: Deploy a GKE Autopilot cluster


### Task 4 : Deploy a sample web application





