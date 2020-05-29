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

I have changed the text in both applications to show the difference. Below you can see results from both applications.

**sample-mvc-v1** 


| ![images/appv1.png](images/appv1.png) |
| ------------------------------------------------------------------- |


**sample-mvc-v2**


| ![images/appv2.png](images/appv2.png) |
| ------------------------------------------------------------------- |

**Docker Preparations**

To dockerize the MVC application, we need the Dockerfile file, as you are familiar with, that we can encode it as follows.

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
