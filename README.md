# .Net core app to GCP using Traefic and LetsEncypt

This repository aims to deploye .Net core mvc application to Google Cloud with Traefic as ingress controller and LetsEncrypt. We will run 2 mvc application in 2 different pods (sample-mvc-v1 & sample-mvc-v2) and redirect the request by using Traefic. We will also use SSL certificate deployment by using DNS challange in LetsEncrypt.

**Prerequisite**

* Google Cloud Account
* Visual studio code
* Docker [Install Docker](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)
* Google Cloud SDK & Kubectl [Install gcloud SDK & kubectl](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)

**Setting up .Net Core Web API**

Let's start with a .net core mvc application as an example. We will create 2 mvc applications for this excersize to demonstrate Traefik. To create it, we can proceed with the terminal command below.

```
dotnet new mvc -o sample-mvc-v1
dotnet new mvc -o sample-mvc-v2
```

This will create a projects with necessory structure. Run the applications by typing following on terminal.

```
dotnet run
```

I have changed the text in both applications to show the difference. Below you can see results from sample-mvc-v1 and sample-mvc-v2


| ![images/appv1.png](images/appv1.png) |
| ------------------------------------------------------------------- |


| ![images/appv2.png](images/appv2.png) |
| ------------------------------------------------------------------- |
