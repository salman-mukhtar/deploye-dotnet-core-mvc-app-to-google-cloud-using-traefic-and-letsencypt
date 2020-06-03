# Deploying .Net Core MVC Web Application to Google Cloud

First of all we need to set up our reverse proxy (Ingress controller) that will handle the in-comming trafic from outside and redirect it to mapped service. To impliment/deploy traefik please follow the **[instructions](traefik/README.md).**

| ![images/traefik.png](images/traefik.png) |
| ------------------------------------------------------------------- |

* Step 01 - List Traefik Directory Files

| ![images/01-listtraefik.png](images/01-listtraefik.png) |
| ------------------------------------------------------------------- |

* Step 02 - Create Configmap

| ![images/02-createconfigmap.png](images/02-createconfigmap.png) |
| ------------------------------------------------------------------- |

* Step 03 - Create Password File

| ![images/03-addingpassword.png](images/03-addingpassword.png) |
| ------------------------------------------------------------------- |

* Step 04 - Create Secret

| ![images/04-createsecret.png](images/04-createsecret.png) |
| ------------------------------------------------------------------- |

* Step 05 - Create Traefik RBAC

| ![images/05-applyrbac.png](images/05-applyrbac.png) |
| ------------------------------------------------------------------- |

* Step 06 - Create Traefik Deployment

| ![images/06-traefikdeployment.png](images/06-traefikdeployment.png) |
| ------------------------------------------------------------------- |

* Step 07 - Create Traefik Service

| ![images/07-traefikservice.png](images/07-traefikservice.png) |
| ------------------------------------------------------------------- |

* Step 08 - Check & Wait For External IP

| ![images/08-trafikexternalip.png](images/08-trafikexternalip.png) |
| ------------------------------------------------------------------- |

* Step 09 - Check & Fix DNS Settings With New IP

| ![images/08-trafikexternalip.png](images/08-trafikexternalip.png) |
| ------------------------------------------------------------------- |

| ![images/dns.png](images/dns.png) |
| ------------------------------------------------------------------- |

* Step 10 - Create Traefik Ingress 

| ![images/09-traefikingress.png](images/09-traefikingress.png) |
| ------------------------------------------------------------------- |


Our applications will be access through following url's

* appv1.digitalbarries.com
* appv2.digitalbarries.com


Now we create 2 deployments for our applications to run in pods on GCP. The deployment files will look as follows.

**Deployment file for mvc application v1**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-mvc-v1
  labels:
    app: sample-mvc
    version: v1
spec:
  selector:
    matchLabels:
      app: sample-mvc
      version: v1
  replicas: 1
  template:
    metadata:
      labels:
        app: sample-mvc
        version: v1
    spec:
      containers:
        - name: sample-mvc
          #imagePullPolicy: Never
          image: mesalman/app:v1
          resources:
            limits:
              memory: "50Mi"
              cpu: "50m"
            requests:
              memory: "20Mi"
              cpu: "20m"
          ports:
          - containerPort: 80
```

**Deployment file for mvc application v2**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-mvc-v2
  labels:
    app: sample-mvc
    version: v2
spec:
  selector:
    matchLabels:
      app: sample-mvc
      version: v2
  replicas: 1
  template:
    metadata:
      labels:
        app: sample-mvc
        version: v2
    spec:
      containers:
        - name: sample-mvc
          #imagePullPolicy: Never
          image: mesalman/app:v2
          resources:
            limits:
              memory: "50Mi"
              cpu: "50m"
            requests:
              memory: "20Mi"
              cpu: "20m"
          ports:
          - containerPort: 80
```

Now we need our services that will be exposed on top of our deployment and let us talk to our applications running in pods.


**Service for mvc application v1**

```
apiVersion: v1
kind: Service
metadata:
  name: sample-mvc-v1
  labels:
    app: sample-mvc
    version: v1
spec:
  ports:
    - port: 80
  selector:
    app: sample-mvc
    version: v1
  type: ClusterIP
```
         
**Service for mvc application v2**

```
apiVersion: v1
kind: Service
metadata:
  name: sample-mvc-v2
  labels:
    app: sample-mvc
    version: v2
spec:
  ports:
    - port: 80
  selector:
    app: sample-mvc
    version: v2
  type: ClusterIP
```
         
Last but not least we needs to great ingress.


**Ingress for mvc application v1**

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: sample-mvc-v1
spec:
  rules:
  - host: appv1.digitalbarries.com
    http:
      paths:
      - path: /
        backend:
          serviceName: sample-mvc-v1
          servicePort: 80
```

**Ingress for mvc application v2**

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: sample-mvc-v2
spec:
  rules:
  - host: appv2.digitalbarries.com
    http:
      paths:
      - path: /
        backend:
          serviceName: sample-mvc-v2
          servicePort: 80
```

We are now ready to deploy our applications to GCP. Change directory to "docker-deployment-v1" and run following commands on terminal


```
kubectl apply -f 01-deployment.yaml
kubectl apply -f 02-service.yaml
kubectl apply -f 03-ingress.yaml
```

| ![images/deployappv1.png](images/deployappv1.png) |
| ------------------------------------------------------------------- |


Change directory to "docker-deployment-v2" and repeat the same step for application v2. 

```
kubectl apply -f 01-deployment.yaml
kubectl apply -f 02-service.yaml
kubectl apply -f 03-ingress.yaml
```

| ![images/deployappv2.png](images/deployappv2.png) |
| ------------------------------------------------------------------- |


Once done you go to google cloud console/dashboard and you will see your pods, services and ingress ready as below.

| ![images/deployment.png](images/deployment.png) |
| ------------------------------------------------------------------- |


| ![images/services-ingress.png](images/services-ingress.png) |
| ------------------------------------------------------------------- |

Last but not the least we now check our applications by using following url's

* appv1.digitalbarries.com
* appv2.digitalbarries.com

The result is as follows.

| ![images/appv1result.png](images/appv1result.png) |
| ------------------------------------------------------------------- |

| ![images/appv2result.png](images/appv2result.png) |
| ------------------------------------------------------------------- |
