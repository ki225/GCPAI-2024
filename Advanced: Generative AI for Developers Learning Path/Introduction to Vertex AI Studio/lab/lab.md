# Objectives

- Analyze images with Gemini
- Explore Vertex AI Studio Freeform mode
- Design text prompts for zero-shot, one-shot, and few-shot prompting
- Generate conversations with chat prompts

# Tasks
## Task 1. Analyze images with Gemini in Freeform mode
### Analyze images with Gemini
1. In the Google Cloud console, from the Navigation menu (Navigation menu), select Vertex AI > Vertex AI Studio > Overview.
2. Under Generate with Gemini, click Open Freeform.
3. On the top left, click Untitled Prompt and rename your prompt as Image Analysis.
4. In the Configuration section on the top right, click on the Model dropdown then select the gemini-1.5-pro model.
5. Download the sample image. Right-click the timetable image and then save it to your desktop.
6. On the top right of the Prompt section, click Insert media > Upload. Upload the timetable image you downloaded. The media can be in the form of an image, video, text, or audio file.
7. The image will be displayed inside of the Prompt section. Copy the following text and paste it under the image and click on the Submit button on the bottom right of the Prompt section.
8. Describe the image. Replace the previous prompt with the following and click the Submit button.
9. Tune the parameter. In the Configuration section, adjust the temperature by scrolling from left (0) to right (2). Resubmit the prompt to observe any changes in the outcome compared to the previous result.



## Task 2. Explore multimodal capabilities
1. Navigate to Cloud Storage > Buckets and copy the name of your Cloud Storage bucket and save it to use in the further step.
2. Click Activate Cloud Shell Activate Cloud Shell icon at the top of the Google Cloud console.
3. In your Cloud Shell terminal, run the command below to copy the sample video gs://spls/gsp154/video/train.mp4 to your Cloud Storage bucket. Replace <Your-Cloud-Storage-Bucket> with the bucket name you copied
```
gcloud storage cp gs://spls/gsp154/video/train.mp4 gs://<Your-Cloud-Storage-Bucket>
```
4. From the Navigation menu (Navigation menu), select Vertex AI > Vertex AI Studio > Overview.
5. Under Generate with Gemini, click Open Freeform.
6. Click Inset Media > Import from Cloud Storage.
7. Click on your bucket name and then click on the sample video i.e., train.mp4 and click Select.
8. Generate information about the video by inserting the following prompt and clicking the Submit button.

## Task 3. Design text prompts
### Prompt design
#### methods
1. Zero-shot prompting - This is a method where the LLM is given only a prompt that describes the task and no additional data. For example, if you want the LLM to answer a question, you just prompt "what is prompt design?".
2. One-shot prompting - This is a method where the LLM is given a single example of the task that it is being asked to perform. For example, if you want the LLM to write a poem, you might give it a single example poem.
3. Few-shot prompting - This is a method where the LLM is given a small number of examples of the task that it is being asked to perform. For example, if you want the LLM to write a news article, you might give it a few news articles to read.
#### parameters
1. Temperature controls the randomness in token selection. A lower temperature is good when you expect a true or correct response. A temperature of 0 means the highest probability token is always selected. A higher temperature can lead to diverse, unexpected, or potentially biased results. The gemini-1.5-pro model has a temperature range of 0 - 2 and a default of 1.
2. Output token limit determines the maximum amount of text output from one prompt. A token is approximately four characters.

>[!Note]
>Note: Temperature controls the degree of randomness in token selection. Lower temperatures are good for prompts that expect a true or correct response, while higher temperatures can lead to more diverse, unexpected, or potentially biased results. With a temperature of 0 the highest probability token is always selected.

Example: Few-shot prompting

## Task 4. Generate conversations





