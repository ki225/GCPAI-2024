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

![](img/img1.png)
![](img/img2.png)
![](img/img3.png)
![](img/img4.png)
![](img/img5.png)

### Task 2. Create a Cloud Workstation
#### Create a workstation configuration
![](img/img6.png)
![](img/img7.png)
![](img/img8.png)


#### Create a workstation
![](img/img9.png)
![](img/img10.png)
![](img/img11.png)
![](img/img12.png)
![](img/img13.png)
![](img/img14.png)
![](img/img15.png)
![](img/img16.png)
![](img/img17.png)

#### Launch the IDE
##### set allow cookie
![](img/img18.png)
![](img/img19.png)
![](img/img20.png)
![](img/img21.png)



### Task 3. Update the Cloud Code extension to enable Gemini
![](img/img22.png)
![](img/img23.png)

### Task 4. Chat with Gemini
#### Connect to Google Cloud
![](img/img24.png)
![](img/img25.png)
![](img/img26.png)
![](img/img27.png)

#### Enable Gemini in Cloud Code
![](img/img28.png)
![](img/img29.png)
![](img/img30.png)

#### chat to Gemini
![](img/img31.png)
![](img/img32.png)

### Task 5. Develop a Python app
#### Create a Python app by using the steps from Gemini
![](img/img33.png)
![](img/img34.png)
![](img/img35.png)
![](img/img36.png)
![](img/img37.png)


#### Explore the app with Gemini
![](img/img39.png)
![](img/img40.png)
![](img/img41.png)

#### Run the app locally
![](img/img42.png)
![](img/img43.png)
![](img/img44.png)
![](img/img45.png)
![](img/img46.png)
![](img/img47.png)
![](img/img48.png)
![](img/img49.png)
![](img/img50.png)




### Task 6. Enhance the Python app
#### Generate sample data using Gemini
![](img/img53.png)
![](img/img54.png)

#### Add the GET /inventory list API method to the app


```py
from flask import Flask, render_template, jsonify
from inventory import inventory
```
![](img/img55.png)

![](img/img56.png)

#### Add the GET /inventory/{productID} method to the app

![](img/img57.png)


#### Rebuild and redeploy the app locally
You can run your app locally from your IDE using the Cloud Run emulator. In this case, locally means on the workstation machine.

1. In the activity bar of your IDE, click Cloud Code (Code OSS Cloud Code menu).
2. In the Cloud Run activity bar, click Run App on Local Cloud Run Emulator (Cloud Run - run on local emulator).
3. When prompted at the top of the screen to Enable minikube gcp-auth addon to access Google APIs, select Yes.

![](img/img58.png)

#### Test the API methods
![](img/img59.png)
![](img/img60.png)
![](img/img61.png)
![](img/img62.png)


### Task 7. Deploy the app to Cloud Run
![](img/img63.png)
![](img/img64.png)
![](img/img65.png)
![](img/img66.png)
![](img/img67.png)
![](img/img68.png)
![](img/img69.png)
![](img/img70.png)
![](img/img71.png)
![](img/img72.png)

To get the URL for the Cloud Run service inventory page, in Cloud Shell, run the following command:
```
export SVC_URL=$(gcloud run services describe hello-world \
  --project set at lab start \
  --region set at lab start \
  --platform managed \
  --format='value(status.url)')
echo ${SVC_URL}/inventory
```

![](img/img73.png)
![](img/img74.png)











