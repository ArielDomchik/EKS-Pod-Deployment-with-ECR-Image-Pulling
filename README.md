EKS+ECR Deployment

Deploying a pod with a service, or deploying a deployment with a service to an EKS cluster, which pulls images from an ECR in AWS.


- Overview

This project provides a simple way to deploy a pod with a service, or a deployment with a service to an EKS cluster, which pulls images from an ECR in AWS. The image being pulled from the ECR is a simple web application that shows the hostname and IP of the host. This project can be easily wrapped for your own CI/CD purposes.
Prerequisites

Before you can deploy the pod or deployment to your EKS cluster, you'll need the following:

    A running EKS cluster
    Access to an ECR repository in your AWS account
    kubectl command-line tool installed


- Deployment

To deploy the pod or deployment to your EKS cluster, simply run the following command:

    kubectl apply -f podspec.yaml -f podservice.yaml

This will create the necessary pod and service resources in your EKS cluster. You can customize the podspec.yaml and podservice.yaml files to fit your own needs.

- Testing LoadBalancer

Once the pod or deployment and service resources are created, you can test the LoadBalancer functionality by running the following command:

kubectl get services

This will output a list of services in your EKS cluster. You can then open a web browser and navigate to http://<external-ip>:80 to see the web application running.
    
- Conclusion :

That's it! With just a few simple steps, you can deploy a pod with a service, or a deployment with a service to your EKS cluster, and test the LoadBalancer functionality. Feel free to customize the podspec.yaml and podservice.yaml files to fit your own needs.
