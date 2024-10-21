# Using Gemini Throughout the Software Development Lifecycle

## Tasks
### Task 1. Configure your environment and account


### Task 2. Create and launch a Cloud workstation
#### View the workstation cluster
1. select More Products > Tools > Cloud Workstations.
2. In the Navigation pane, click Cluster management.
3. Check the Status of the cluster. If the status of the cluster is Reconciling or Updating, periodically refresh and wait until it becomes Ready before moving to the next step.

#### Create a workstation configuration

click Workstation configurations, and then click Create Workstation Configuration.

#### Create a workstation
click Workstations, and then click Create Workstation.

To start the workstation, click Start.
#### Launch the IDE
1. To enable third-party cookies in Chrome, in the Chrome menu, click Settings.
2. In the search bar, type Third-party cookies.
3. Click the Third-party cookies setting, and select Allow third-party cookies.
4. To launch the Code OSS IDE on the workstation, from the Workstations page in the Google Cloud console, click Launch.

### Task 3. Update the Google Cloud Code extension to enable Gemini
#### Update Google Cloud Code
1. In the IDE activity bar, click Extensions (Code OSS Extensions menu).
2. In the Extensions search bar, type google cloud code, and press ENTER.
3. When the results generate, find Gemini Code Assist + Google Cloud Code in the list of extensions.
4. If the button on the extension says Update, click Update.
5. If the update was required, when Cloud Code finishes updating in the workstation, click Reload Required.

#### Connect to Google Cloud

#### Enable Gemini in Google Cloud Code

### Task 4. Build the web app
#### Configure the environment
set project ID
```
gcloud config set project qwiklabs-gcp-04-392c83f32fdf
```
run the Docker credential helper
```
gcloud auth configure-docker
```
access Cloud Storage
```
gcloud auth login
```
download the `cymbal-superstore` application code
```
gcloud storage cp -r gs://cloud-training/OCBL435/cymbal-superstore .
```
#### Build the backend
1. To build the backend container image, in the terminal of your Workstation IDE, run the following commands:
```
cd ~/cymbal-superstore/backend
docker build --platform linux/amd64 -t gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api .
```
2. To push the built backend image to the container registry repository, run the following command
```
docker push gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api
```
3. To deploy the backend as a service on Cloud Run, run the following command
```
gcloud run deploy inventory --image=gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api --port=8000 --region=us-west1 --set-env-vars=PROJECT_ID=qwiklabs-gcp-04-392c83f32fdf --allow-unauthenticated
```
4. Copy the value of the Service URL that is displayed in the output of the gcloud run deploy command.

#### Build the frontend
1. Update the frontend code to connect to your backend Cloud Run endpoint by editing the file frontend/.env.production in the IDE:

a. In the IDE activity bar, click Explorer (Code OSS Explorer menu), and then click Open Folder.
b. In the folder list, select cymbal-superstore, and then click Ok.
c. Expand the frontend folder, and select the file .env.production.
d. In the file, replace the value of REACT_APP_INVENTORY_API_URL by pasting the value of your Cloud Run backend service endpoint URL that you copied earlier.
e. Save the changes to the file.

2. To build the frontend, open a new terminal window, and then run the following commands:
```
cd ~/cymbal-superstore/frontend
npm install && npm run build
```
3. To upload the frontend web app to Cloud Storage, run the following command:
```
gcloud storage cp -r build/* gs://qwiklabs-gcp-04-392c83f32fdf-cymbal-frontend
```

#### View the web app
See the web app with url `http://34.160.12.3`

### Task 5. Modify the web app backend
#### Develop the /newproducts endpoint
1. In the Cloud Code editor, open the file backend/index.ts by clicking Explorer (Code OSS Explorer menu), and then click Open Folder.
2. In the index.ts code file, scroll to line 91 where you see the placeholder comment for the /newproducts endpoint:
```
// Your code for the GET /newproducts endpoint goes here.
```
3. Replace the placeholder comment with the following Gemini prompt:
```
// Create a new endpoint /newproducts that uses where filters to retrieve only products that were added within the last seven days and are in stock.
```
4. To prompt Gemini to generate the function code, select the entire comment, and then click the bulb (Code OSS Gemini bulb).
5. In the More Actions menu, select Gemini: Generate code.

>[!Warning]
>記得從右下角點 Gemini 然後開啟他，選擇project id
>這樣才能生成code

The generated code should be similar to the following:
```
app.get("/newproducts", async (req: Request, res: Response) => {
  const products = await firestore
    .collection("inventory")
    .where("timestamp", ">", new Date(Date.now() - 604800000))
    .where("quantity", ">", 0)
    .get();
  const productsArray: any[] = [];
  products.forEach((product) => {
    const p: Product = {
      id: product.id,
      name: product.data().name,
      price: product.data().price,
      quantity: product.data().quantity,
      imgfile: product.data().imgfile,
      timestamp: product.data().timestamp,
      actualdateadded: product.data().actualdateadded,
    };
    productsArray.push(p);
  });
  res.send(productsArray);
});
```

### Task 6. Test the app locally
#### Set the PORT environment variable
1. In the IDE menu (Code OSS main menu), select View > Command Palette....
2. In the command palette, type user settings json, and, from the list, select Open User Settings (JSON).
3. Update the json object by adding the following attribute:
```
"terminal.integrated.env.linux": {
    "PORT": "8000"
},
```
#### Run the backend server
authenticate the application to access Firestore
```
gcloud auth application-default login
```
start the backend server
```
cd ~/cymbal-superstore/backend
npm run start
```
#### Invoke the /newproducts endpoint
1. To invoke the API endpoint, run the following command. The command responds with an error
```
curl localhost:8000/newproducts
```
2. Switch back to the original terminal window to view the exception trace that is logged from the backend. From the trace, it seems that the root cause of the exception is:
```
details: 'Cannot have inequality filters on multiple properties: [quantity, timestamp]',
```
### Task 7. Fix the backend with help from Gemini
#### Get help from Gemini
ask Gemini
```
Help me debug this Firestore client error: Cannot have inequality filters on multiple properties
```
response
```
The error message "Cannot have inequality filters on multiple properties" in Firestore means you are trying to perform a query that violates a Firestore limitation: You cannot have range (<, <=, >, >=) or not equals (!=) filters on more than one property in a single query.
```
In the backend/index.ts file, to remove the quantity inequality filter, delete the following code from the /newproducts handler in the backend/index.ts file:
```
# By deleting this code, products with a quantity of 0 will not be filtered from the API response, and there is only one inequality filter.

.where("quantity", ">", 0)
```
To remove products that have a quantity of 0, wrap the code in products.forEach() with the following if statement:
```
if (product.data().quantity > 0) {
}
```
Finnally:
```
app.get("/newproducts", async (req: Request, res: Response) => {
  const sevenDaysAgo = new Date();
  sevenDaysAgo.setDate(sevenDaysAgo.getDate() - 7);
  const products = await firestore.collection("inventory").where("timestamp", ">=", sevenDaysAgo).get();
  const productsArray: any[] = [];
  
  products.forEach((product) => {
    if (product.data().quantity > 0) {
      const p: Product = {
        id: product.id,
        name: product.data().name,
        price: product.data().price,
        quantity: product.data().quantity,
        imgfile: product.data().imgfile,
        timestamp: product.data().timestamp,
        actualdateadded: product.data().actualdateadded,
      };
      productsArray.push(p);
    }
    
    
  });
  res.send(productsArray);
});
```

#### Re-test the API
To restart the backend API server, in a terminal, run the following commands:
```
cd ~/cymbal-superstore/backend
npm run start
```
Switch to the other bash terminal, and then run the curl command again:
```
curl localhost:8000/newproducts
```
### Task 8. Test the frontend
#### Rebuild and redeploy the backend
Follow the previous steps to rebuild the updated backend service container image, push the image to the registry, and redeploy the backend service to Cloud Run. The steps are repeated here for convenience.

1. In the IDE, open a new terminal, or use the terminal that is already open.
2. To build the backend container image, push it to the repository, and deploy the backend service on Cloud Run, run the following commands:
```
cd ~/cymbal-superstore/backend
docker build --platform linux/amd64 -t gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api .
docker push gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api
gcloud run deploy inventory --image=gcr.io/qwiklabs-gcp-04-392c83f32fdf/cymbal-inventory-api --port=8000 --region=us-west1 --set-env-vars=PROJECT_ID=qwiklabs-gcp-04-392c83f32fdf --allow-unauthenticated
```
#### Test the web app
1. Open a tab in your browser, and then navigate to the following URL:

### Task 9. Develop tests with help from Gemini
#### Develop the test
1. In your Workstation IDE, open the file backend/index.test.ts.
2. To open the Gemini chat pane, in the activity bar of your IDE, click Gemini (Code OSS Gemini menu).
3. To prompt Gemini to help you write a test for the GET endpoint /newproducts, type the prompt below, and click Send (Gemini send):
  - Q
  ```
  Help me write an Express.js test using Jest, in typescript, for the GET /newproducts handler in index.ts. The test should check if the response code is 200 and the list of new products is length 8.
  ```
  - A
  ```
  import request from "supertest";
  import app from "./index";
  
  describe('GET /newproducts', () => {
    it('should return a 200 status code and a list of 8 new products', async () => {
      const response = await request(app).get('/newproducts');
  
      expect(response.statusCode).toBe(200);
      expect(response.body).toHaveLength(8); 
    });
  });
  ```


4. Gemini responds with the generated test code. Copy and paste the describe code block into the backend/index.test.ts file.

#### Run the test
1. To execute the tests, run the following commands:
```
cd ~/cymbal-superstore/backend
npm run test
```

### Task 10. Use Gemini with BigQuery
#### Upload data to BigQuery
```
What bq command can be used to upload CSV data from Cloud Storage to BigQuery?
```
```
bq load --source_format=CSV \
  myproject.mydataset.mytable \
  gs://mybucket/mydata.csv
```
Update the default query to:

```
SELECT * FROM `qwiklabs-gcp-04-392c83f32fdf.cymbal_sales.cymbalsalestable` LIMIT 1000
```

#### Generate a SQL query with help from Gemini
In the Query builder menu, click Gemini (magic pencil), and enable Auto-completion.

```
SELECT SUM(PRICE_PER_UNIT * QUANTITY_SOLD_AUG_5) AS total_aug_5 FROM `qwiklabs-gcp-04-392c83f32fdf.cymbal_sales.cymbalsalestable`;
# Get sales for total_aug_12
SELECT
    sum(PRICE_PER_UNIT * QUANTITY_SOLD_AUG_12) AS total_aug_12
  FROM
    `qwiklabs-gcp-04-392c83f32fdf.cymbal_sales.cymbalsalestable`;
```


