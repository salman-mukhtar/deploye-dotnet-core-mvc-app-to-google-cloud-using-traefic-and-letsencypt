# .Net core app to GCP using Traefic and LetsEncypt

This repository aims to deploye .Net core mvc application to Google Cloud with Traefic as ingress controller and LetsEncrypt. We will run 2 mvc applications in 2 different pods (sample-mvc-v1 & sample-mvc-v2) and redirect the request by using Traefic. We will also use SSL certificate by using HTTP challange in LetsEncrypt.

#Prerequisite

* Google Cloud Account
* Dockerhub Account
* Visual Studio Code
* Docker Engine [Install Docker](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)
* Google Cloud SDK & Kubectl [Install gcloud SDK & kubectl](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)

#Setting up .Net Core Web API

Let's start with a .net core mvc application as an example. We will create 2 mvc applications for this excersize to demonstrate Traefik. To create mvc applications, we can proceed with the terminal command below.

```
dotnet new mvc -o sample-mvc-v1
dotnet new mvc -o sample-mvc-v2
```

This will create a projects with necessory structure. Run the applications by typing following on terminal.

```
dotnet run
```

I have changed the text in both applications to show the difference. Below you can see results from both applications.

**sample-mvc-v1** 


| ![images/appv1.png](images/appv1.png) |
| ------------------------------------------------------------------- |


**sample-mvc-v2**


| ![images/appv2.png](images/appv2.png) |
| ------------------------------------------------------------------- |

#Docker Preparations

To dockerize the MVC applications, we need the Dockerfile file, as you are familiar with, that we can encode it as follows.

**Dockerfile for sample-mvc-v1**

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY sample-mvc-v1/*.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY sample-mvc-v1/. ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "sample-mvc-v1.dll"]
```

**Dockerfile for sample-mvc-v2**

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY sample-mvc-v2/*.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY sample-mvc-v2/. ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "sample-mvc-v2.dll"]
```

After completing the dockerfile file, we can start dockerizing both applications and build the images. Use the build command as follows.

```
docker build -t mesalman/app:v1 -f docker-deployment-v1/Dockerfile .
docker build -t mesalman/app:v2 -f docker-deployment-v2/Dockerfile .
```

This will create 2 docker images tagged with "mesalman/app:v1" and "mesalman/app:v2" respectively. See below.


**Docker image for mesalman/app:v1**

| ![images/imagev1.png](images/imagev1.png) |
| ------------------------------------------------------------------- |


**Docker image for mesalman/app:v2**

| ![images/imagev2.png](images/imagev2.png) |
| ------------------------------------------------------------------- |

Now its time to push the docker images to dockerhub as public images so that we can pull them on Google Cloud while deploying our applications. First we login to dockerhub if not already. Use following command on terminal.

```
docker login --username=mesalman
```

It will ask your password. Write password and hit enter. You will see something like below.

| ![images/dockerhub-login.png](images/dockerhub-login.png) |
| ------------------------------------------------------------------- |


Check the images ID using  

```
docker images
```

and what you will see will be similar to

| ![images/dockerimages.png](images/dockerimages.png) |
| ------------------------------------------------------------------- |


To push the images to dockerhub use following commands on terminal

```
docker push mesalman/app:v1
docker push mesalman/app:v2
```

Now you can see images are uploaded to dockerhub.


| ![images/dockerhub-images.png](images/dockerhub-images.png) |
| -------------------------------------


# Deploying MVC webapplication to Google Cloud
