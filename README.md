Deploying Python web application to your EKS Cluster!

This sample-app is to test and watch your hostnames and ip's of the pods.

- Clone the repository : https://github.com/ArielDomchik/Leumi-1.git

- Apply Configuration files at /k8s-configuration, There are 2 options:

- podspec.yaml and podservice.yaml to deploy a pod only. (run kubectl apply -f podspec.yaml podservice.yaml)

- at /configuration-for-deployment you can find deployment.yaml and service.yaml (this is to test the cluster's loadbalancer) 

- Also you have the option to wrap the project for your own CI/CD Purposes
