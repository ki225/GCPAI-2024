# Implement continuous delivery with Gemini

## Tasks
### Task 1. Configure your environment and account for Gemini

### Task 2. Configure Google Kubernetes Engine
enable the GKE API
```
gcloud services enable container.googleapis.com --project ${PROJECT_ID}
```
grant your user admin permissions for GKE
```
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/container.admin
```
ask Gemini
```
Q:
What's the gcloud command for creating a zonal GKE cluster with a custom number of nodes and custom machine type?

A:
gcloud container clusters create CLUSTER_NAME \
    --zone ZONE \
    --machine-type MACHINE_TYPE \
    --num-nodes NUM_NODES
```
create a zonal GKE cluster
```
gcloud container clusters create test \
    --project=qwiklabs-gcp-02-bc92530d542f \
    --zone=us-central1-a \
    --num-nodes=3 \
    --machine-type=e2-standard-4
```

### Task 3. Use kubectl to deploy a set of microservices
clone the repository
```
git clone --depth=1 https://github.com/GoogleCloudPlatform/microservices-demo
```
deploy the microservices to GKE
```
cd ~/microservices-demo
kubectl apply -f ./release/kubernetes-manifests.yaml
```
To check the deployment status
```
kubectl get deployments
```
To determine the URL for the application
```
echo "http://$(kubectl get service frontend-external -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')"
```

### Task 4. Use Gemini to understand logs for workloads in GKE
1. To open Logs Explorer, in the Google Cloud console, select the Navigation menu (Navigation menu icon), and then select View All Products > Observability > Logging.

2. get query

send prompt: What is a Logs Explorer query to search for logs from Pods in a namespace called "default" in a GKE cluster named "test"?
```
resource.type="k8s_container"
resource.labels.kubernetes.io/namespace="default"
resource.labels.kubernetes.io/cluster="test"
```

3. In the Query box, paste the query, and then click Run query.



4. To learn about a log entry, expand a log entry, and then click Explain this log entry.

### Task 5. Use Gemini to create a private build environment for container images
send prompt: What is a Cloud Build worker pool?

response:
```
A Cloud Build worker pool is a group of virtual machines (VMs) that are dedicated to running your Cloud Build builds. They provide a way to customize the environment where your builds run, including the machine type, disk size, and network configuration.

Here are some key things to know about Cloud Build worker pools:

- Private Pools: Worker pools are private, meaning they are only accessible to your project. This helps to ensure that your builds are isolated from other users and that your data is secure.
- Customization: You can customize the worker pool to meet the specific needs of your builds. For example, you can choose a machine type with more memory or CPU, or you can add additional disks for storage.
- Control: Worker pools give you more control over the environment where your builds run. This can be helpful if you need to use specific software or libraries that are not available in the default Cloud Build environment.
- Performance: Worker pools can improve the performance of your builds by providing dedicated resources. This can be especially helpful for large or complex builds.

There are two main types of worker pools:

1. Default Worker Pools: These are the standard worker pools that are used by Cloud Build. They are automatically provisioned and managed by Google.
2. Private Worker Pools: These are worker pools that you create and manage yourself. They give you more control over the environment where your builds run.
3. Overall, Cloud Build worker pools are a powerful tool that can help you to improve the performance, security, and control of your builds.
```
send prompt: Can you create a private worker pool that has no access to the public internet?

response
```
Yes, you can create a private worker pool that has no access to the public internet. This is done by configuring the worker pool to not have any external IP addresses assigned to it.

Here's how you can do it:

1. Create a Private Pool: When creating a private worker pool, you'll need to specify the VPC network it will be connected to.
2. Disable External IPs: During the creation process, there will be an option to "Assign external IPs". Make sure to uncheck this option. This will prevent the worker pool from having any public IP addresses.
3. Configure Firewall Rules: You can further restrict access by configuring firewall rules on your VPC network to block any outbound traffic to the public internet.

By following these steps, you can create a private worker pool that is isolated from the public internet, ensuring that your builds only have access to resources within your private network.
```

ask: If I create a private pool with no public egress, can I use Artifact Registry to host my packages privately and access them from a private pool?

response
```
Yes, you can absolutely use Artifact Registry to host your packages privately and access them from a private worker pool with no public egress. Here's how it works:

1. Private Artifact Registry: You create a private Artifact Registry repository within your Google Cloud project. This repository will store your packages, and you can control access to it using IAM roles.
2. Private Worker Pool: You create a private worker pool within your VPC network, ensuring it has no public IP addresses. This isolates the worker pool from the public internet.
3. Service Account: You create a service account for your worker pool and grant it the necessary permissions to access the private Artifact Registry repository.
4. Network Connectivity: You configure your VPC network to allow the worker pool to communicate with the Artifact Registry service. This can be done using a private service access or by setting up a private network connection.
5. Build Configuration: In your Cloud Build configuration, you specify the service account and the Artifact Registry repository URL. This allows the worker pool to authenticate and download packages from the private repository during your builds.

By following these steps, you can securely access your private packages from a private worker pool without exposing them to the public internet. This ensures that your packages remain secure and only accessible to authorized users within your project.
```

ask: What is the gcloud command for creating a private worker pool with no public egress?

response
```
gcloud builds worker-pools create my-private-pool \
  --location=us-central1 \
  --network=projects/my-project-id/global/networks/my-vpc-network \
  --no-external-ip \
  --machine-type=n1-standard-1
```

So, run the command below:
```
gcloud builds worker-pools create pool-test \
  --project=qwiklabs-gcp-02-bc92530d542f \
  --region=us-central1 \
  --no-public-egress
```

It will show the error msg like:
```
```

Therefore, ask the Gemini: How can I use gcloud to create a private Docker repository for container images in Artifact Registry?

reponse from Gemini is: You can use the gcloud artifacts repositories create command to create a private Docker repository in Artifact Registry. Here's how:

1. Choose a repository name and location:
    - Repository name: This should be a unique name for your repository.
    - Location: This is the region where you want to create the repository.
    - Run the gcloud command:
    ```
    gcloud artifacts repositories create <repository-name> \
        --repository-format=docker \
        --location=<location> \
        --project=<project-id>
    ```
    - Verification:
   ```
   gcloud artifacts repositories list --project=<project-id>
   ```

Therefore, we create the repository by the following commands:

```
gcloud artifacts repositories create my-repo \
  --repository-format=docker \
  --location=us-central1 \
  --description="My private Docker repository"
```

