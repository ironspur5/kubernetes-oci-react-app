# Deploying a Dockerized React App to Kubernetes Cluster managed by Oracle Container Engine for Kubernetes (OKE)

## Prerequisites
* Tutorial was done on macOS.
* Tutorial assumes you have Homebrew installed on you computer. If not, you can install here: https://brew.sh
* Tutorial assumes you have an Oracle Cloud Infrastructure (OCI) account. If not, you can sign up here: https://myservices.us.oraclecloud.com/mycloud/signup?sourceType=_ref_coc-asset-opcSignIn&language=en_US
* Tutorial assumes you have a Docker Hub account. If not, please go here to make one: https://hub.docker.com
* Tutorial assumes you have Docker installed on your computer. If not, you can install it here: https://docs.docker.com/docker-for-mac/install/

## Step 1: Dockerize app 

### Build Docker image
```
$ cd ~/kubernetes-oci-react-app
$ docker build -t <your_name>/ocikubernetesapp .
```

### Run image locally to make sure it works
```
$ docker run -d -p 3000:3000 <your_name>/ocikubernetesapp:latest
```

### Open localhost:3000 to see running, then stop the container
```
$ docker ps
$ docker stop <CONTAINER ID>
```

### Push image to Docker Hub
```
$ docker images
$ docker push <REPOSITORY>
```

### Clean up local machine images and containers 
```
$ docker rm $(docker ps -a -q)
$ docker rmi $(docker images -q)
```

## Step 2: Create Kubernetes Cluster in OCI

### Log into OCI dashboard and add following policy to your compartment
Allow service OKE to manage all-resources in tenancy
* For help in this step refer here: https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengpolicyconfig.htm

### Go to Developer Services > Container Clusters (OKE) to create Kubernetes cluster 
You can choose Quick Create or Custom Create; the only difference is that Custom assumes you have an existing network 

### Open cluster and click "Getting started" on the bottom left of the page to view instructions on how to connect to the cluster from your computer using kubectl
* On you personal computer install Kubernetes Client (kubectl) CLI: ```$ brew install kubectl```

## Step 3: Deploy app to Kubernetes Cluster 

### Build a config file for the deployment of the image onto the cluster 
```
$ kubectl create -f k8deployment.yaml --save-config
```

### View Kubernetes deployments to see if the app was deployed to your cluster and see status 
```
$ kubectl get deployments
```

### View Kubernetes pods to verify pods/replicas are running 
```
$ kubectl get pods
```

### Verify logs in pod to see if the server for React app started 
```
$ kubectl logs POD NAME
```

### Expose cluster to the internet 
```
$ kubectl expose deployment SERVICE NAME --type="LoadBalancer"
```

### View your web app on the service by visiting http://EXTERNAL-IP:PORT
- You can view what the final web app looks like here: http://150.136.132.121:3000/

## Closing Remarks
- Tutorial was built following steps from this Medium article: https://medium.com/faun/how-to-deploy-a-express-node-js-app-on-kubernetes-and-an-intro-to-containerisation-205b5c647426\

















