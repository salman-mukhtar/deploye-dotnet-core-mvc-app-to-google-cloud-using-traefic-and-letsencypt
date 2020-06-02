# Preparing Google Cloud

Now we are ready to deploy our applications to GCP. To proceed we need to have following setup

* **[Create a Google Account](https://console.cloud.google.com)**
* **Create a Project**
  * Go to the Manage resources page in the Cloud Console
  * On the Select organization drop-down list at the top of the page, select the organization in which you 
  want to create a project. If you are a free trial user, skip this step, as this list does not appear.
  * Click Create Project.
  * In the New Project window that appears, enter a project name and select a billing account as applicable. A project name can contain only letters, numbers, single quotes, hyphens, spaces, or exclamation points, and must be between 4 and 30 characters.
  * If you want to add the project to a folder, enter the folder name in the Location box.
  * When you're finished entering new project details, click Create. 


| ![images/newproject1.png](images/newproject1.png) |
| -------------------------------------


| ![images/newproject2.png](images/newproject2.png) |
| -------------------------------------

* **Create a Cluster**
    * Visit the Google Kubernetes Engine menu in Cloud Console.
    * Click the Create cluster button.
    * In the Cluster basics section, complete the following:
      * Enter the Name for your cluster.
      * For the Location type, select Zonal, and then select the desired zone for your cluster.
    * From the navigation pane, under Node Pools, click default-pool.
    * In the Node pool details section, complete the following:
      * Enter a Name for the default Node pool.
      * Choose the Node version for your nodes.
      * Enter the Number of nodes to create in the cluster. You must have available resource quota for the nodes and their resources (such as firewall routes).
	* From the navigation pane, under Node Pools, click Nodes.
	* From the Image type drop-down list, select the desired node image.
	* Choose the default Machine configuration to use for the instances. Each machine type is billed differently. The default machine type is n1-standard-1. 
	* From the Boot disk type drop-down list, select the desired disk type.
	* Enter the Boot disk size.
	* Click Create.
	
| ![images/step1-cluster.png](images/step1-cluster.png) |
| -------------------------------------
	
| ![images/step2-cluster.png](images/step2-cluster.png) |
| -------------------------------------
	
| ![images/step3-cluster.png](images/step3-cluster.png) |
| -------------------------------------
	
| ![images/step4-cluster.png](images/step4-cluster.png) |
| -------------------------------------
	
| ![images/step5-cluster.png](images/step5-cluster.png) |
| -------------------------------------
	
| ![images/step6-cluster.png](images/step6-cluster.png) |
| -------------------------------------
	

Now we are ready to connect to our newly created cluster. To do so please follow these steps.


* **Logging/Connect in to a Cluster**

Now we need to give **"kubectl"** access so that we can run commands.
To log in to a cluster, perform the following steps:

* Visit the GKE menu in Cloud Console.
* From the list of clusters, click the Connect button beside the registered cluster.

Choose how you'd like to log in:

* If you are using a KSA token to log in, select Token, fill the Token field with the KSA's bearer token, and then click Login.
* If you are using basic authentication, select Basic authentication, fill the Username and Password fields, and then click Login.
* If you are using OpenID Connect (OIDC), select OpenID Connect, then click Login.
* If you authenticate successfully, you are able to inspect the cluster and get details about its nodes.

| ![images/step6-cluster.png](images/step6-cluster.png) |
| -------------------------------------
	
| ![images/connect-gcloud.png](images/connect-gcloud.png) |
| -------------------------------------
	
Copy the command and run it in terminal or in cloud shell.
Check the context to be sure you are connected with the Cluster. To do so run following command on terminal.

```
kubectl config get-contexts
```

The name of the context is made of Project ID-Zone-Cluster Name You will see something like follwoing.

| ![images/cluster-context.png](images/cluster-context.png) |
| -------------------------------------


**(To do)**

Now, we move forward to deploye our application to newly created cluster. 
The steps to do that are here: Deploying .Net Core MVC Web Application to Google Cloud [Part 3](PART-3.md)


