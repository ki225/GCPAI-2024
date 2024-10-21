# Introduction to Function Calling with Gemini
## Tasks
### Task 1. Open the notebook in Vertex AI Workbench
### Task 2. Set up the notebook
1. Click on the intro_function_calling-v1.0.0.ipynb file.
2. run through the notebook cells to see how to use the Vertex AI Gemini API with the Vertex AI SDK for Python.
### Task 3. Using function calling for structured Google Store queries
When working with a generative text model, it can be difficult to coerce the LLM to give consistent responses in a structured format such as JSON. Function calling makes it easy to work with LLMs via prompts and unstructured inputs, and have the LLM return a structured response that can be used to call an external function.

You can think of function calling as a way to get structured output from user prompts and function definitions, use that structured output to make an API request to an external system, then return the function response to the LLM to generate a response to the user. In other words, function calling in Gemini extracts structured parameters from unstructured text or messages from users. In this example, you'll use function calling along with the chat modality in the Gemini model to help customers get information about products in the Google Store.
### Task 4. Using function calling to geocode addresses with a maps API
In this example, you'll use the text modality in the Gemini API to define a function that takes multiple parameters as inputs. You'll use the function call response to then make a live API call to convert an address to latitude and longitude coordinates.

run through the notebook cells to see how to use the Gemini Pro model to generate a function call to geocode an address
### Task 5. Using function calling for entity extraction
In the previous examples, you made use of the entity extraction functionality within Gemini Function Calling so that you could pass the resulting parameters to a REST API or client library. However, you might want to only perform the entity extraction step with Gemini Function Calling and stop there without actually calling an API. You can think of this functionality as a convenient way to transform unstructured text data into structured fields.

In this example, you'll build a log extractor that takes raw log data and transforms it into structured data with details about error messages.

