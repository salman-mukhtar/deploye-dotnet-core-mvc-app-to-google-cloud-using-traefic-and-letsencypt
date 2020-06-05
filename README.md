# .Net core app to GCP using Traefic and LetsEncypt

This repository aims to deploye .Net core mvc application to Google Cloud with Traefic as ingress controller and LetsEncrypt. We will run 2 mvc applications in 2 different pods (sample-mvc-v1 & sample-mvc-v2) and redirect the request by using Traefic. We will also use SSL certificate by using HTTP challange in LetsEncrypt.

# Prerequisite

* Google Cloud Account
* Dockerhub Account
* Visual Studio Code
* Docker Engine [Install Docker](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)
* Google Cloud SDK & Kubectl [Install gcloud SDK & kubectl](https://github.com/salman-mukhtar/setting-up-kubernetes-environment/blob/master/README.md)

This document is divided into several parts:

* [Part 1](PART-1.md) - Setting up .Net Core MVC Web Application
* [Part 2](PART-2.md) - Preparing Google Cloud
* [Part 3](PART-3.md) - Deploying .Net Core MVC Web Application to Google Cloud

