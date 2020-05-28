# .Net core app to GCP using Traefic and LetsEncypt

This repository aims to deploye .Net core mvc application to Google Cloud with Traefic as ingress controller and LetsEncrypt. We will run 2 mvc application in 2 different pods (sample-mvc-v1 & sample-mvc-v2) and divert the request by using Traefic. We will also use SSL certicate deployment by using DNS challange in LetsEncrypt.
