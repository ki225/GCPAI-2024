# Develop GenAI Apps with Gemini and Streamlit: Challenge Lab

## Challenge scenario
You onboarded at Cymbal Health just a few months ago. Cymbal Health is an established health network in East Central Minnesota dedicated to reimagining and transforming the way that healthcare can be delivered. Cymbal Health connects care and coverage under one health plan to make it easier for patients to receive high quality care at an affordable cost.

As a value added service, Cymbal Health is interested in improving customer Healthy Living and Wellness, with tips, and advice within apps. One particular area they want to focus on is improving customer nutrition.

cymbal labs logo

By harnessing the power of the Gemini Pro, a multimodal model for generating text, audio, images, and video, Cymbal Health can build applications that generate meal recommendations for its customers.

As an example, your team has been working to create a AI-Based Chef app, that generates recipes based upon customer cuisine preferences, dietary restrictions, food allergies, and what they typically have in their homes or can purchase at a grocery store. Your job is to build, test and deploy a Proof Of Concept (POC) for this Chef app built on the Gemini pro model, Streamlit framework, and Cloud Run. As part of this POC, they have a list of tasks they would like to see you do in an allotted period of time in a sandbox environment.

## Tasks
### Task 1. Use cURL to test a prompt with the API

1. Access Vertex AI Workbench User-Manged Notebook provided to you within the Cloud console in the lab environment.
2. Open a terminal, and download the prompt.ipynb file from Cloud Storage using the command below:
```
gsutil cp gs://spls/gsp517/prompt.ipynb .
```
3. Modify prompt.ipynb to include your project_ID and region within cell 3. You can get these in the left panel of the lab instructions.
4. Modify prompt.ipynb to use the following prompt with cURL within cell 5, by replacing the existing prompt.
```
prompt:
I am a Chef.  I need to create Japanese recipes for customers who want low sodium meals. However, I do not want to include recipes that use ingredients associated with a peanuts food allergy. I have ahi tuna, fresh ginger, and edamame in my kitchen and other ingredients. The customer wine preference is red. Please provide some for meal recommendations. For each recommendation include preparation instructions, time to prepare and the recipe title at the begining of the response. Then include the wine paring for each recommendation. At the end of the recommendation provide the calories associated with the meal and the nutritional facts.
```
### Task 2. Write Streamlit framework and prompt Python code to complete chef.py
For this task you will clone a GitHub repo, and download the chef.py file. Then you will add Streamlit framework code in the chef.py file for the wine preference, to complete the user interface for the application. You will also include a custom Gemini prompt (similar to the one in task 1), but this one includes variables.

Using Cloud Shell clone the repo below from the default directory.
```
git clone https://github.com/GoogleCloudPlatform/generative-ai.git
```
Navigate to the gemini-streamlit-cloudrun directory.
```
cd generative-ai/gemini/sample-apps/gemini-streamlit-cloudrun
```
Download the chef.py file using the following command.
```
gsutil cp gs://spls/gsp517/chef.py .
```
Open the chef.py file in the Cloud Shell Editor and review the code.

Add Streamlit framework radio button option for the wine variable. Include options for Red, White and None.

### Task 3. Test the application


