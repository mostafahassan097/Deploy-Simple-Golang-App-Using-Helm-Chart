# Deploy A Simple GoLang WebApp Using K8s and Packaging Into Helm Chart

## Requirements 
- Make sure that have your own setup of K8s cluster on any cloud provider i will use GCP GKE.
- You have docker on your local setup ([Link](https://docs.docker.com/engine/install/ubuntu/)).
- Install kubectl on local setup by this command ``` sudo apt-get install kubectl ```
- Make sure that you helm on your local setup  ([Link](https://helm.sh/docs/intro/install/)).
- Make sure you have gcloud cli tool if you will use GKE as K8s Cluster ([Link](https://cloud.google.com/sdk/docs/install#deb)).
```
For ubuntu
 curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
 sudo apt-get install apt-transport-https --yes
 echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
 sudo apt-get update
 sudo apt-get install helm
 ```


# Steps To Deploy App On K8s Cluster
###  1- Clone the Repo
```
       git clone git@github.com:mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster.git
      
       cd Deploy-Simple-Golang-App-On-K8s-Cluster
```
###  2- Dockerize  Steps
```
     docker build -t {docker_id}/go-web-app:v1 .
     docker login
     docker push {docker_id}/go-web-app:v1
```
###  3- Create GKE Cluster On GCP as Shown Below
#### Kubernetes Engine > Clusters > Create > Configure Standard
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/1.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/2.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/3.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/4.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/5.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/6.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/7.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/8.png)
###  4- Connect Cluster to you local setup by the below command
 ``` 
 gcloud container clusters get-credentials {YOUR-CLUSTER_NAME} --zone {ZONE} --project {PROJECT_ID} 
 ```

### 5- Create K8s Resources by K8s yaml files
```sh
 cd Kubernetes
 kubectl apply -f  go-web-deploy.yaml # Deployment 
 kubectl apply -f  go-lb-service.yaml # Service
```
- Check Resources 
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/10.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/11.png)


### 6- Create Deployment Using Helm Chart Package
- Clean Created Resources
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/15.png)

- Steps To Create Helm Chart From Kubernetes Resources

1- Run ` helm create my-helm-chart ` in your working dir.

2- Then  run ` cd my-helm-chart/templates `

3- Edit Both Files deployment , values as same values in K8s yaml files
- templates/Deployment 
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/14.png)
- values.yaml Edit Values (Image , Service and Port)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/12.png)
![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/13.png)

4- Checking chart by run `helm lint {NAME_OF_CHART}` that has no errors.

5- Deploy by run `helm install {NAME} {CHART}`

![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/16.png)

![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/17.png)

6- Access to LoadBalancer IP from Browser 

![App Screenshot](https://github.com/mostafahassan097/Deploy-Simple-Golang-App-On-K8s-Cluster/blob/main/Screenshots/18.png)

# ([Demo](http://34.132.64.90/))
