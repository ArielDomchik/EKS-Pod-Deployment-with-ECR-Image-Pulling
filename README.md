- EKS+ECR Deployment

This project provides a simple way to deploy a pod with a service, or a deployment with a service to an EKS cluster, which pulls images from an ECR in AWS. The image being pulled from the ECR is a simple web application that shows the hostname and IP of the host. This project can be easily wrapped for your own CI/CD purposes.
Prerequisites

Before you can deploy the pod or deployment to your EKS cluster, you'll need the following:

    A running EKS cluster
    Access to an ECR repository in your AWS account
    kubectl command-line tool installed

- Setup

Here are the steps you need to follow to set up this project:

    - Create a GitLab user and repository for the project
    - Import a public SSH key to the GitHub project to allow accessibility
    - Upload the project to the GitLab repository
    - Install Jinja2=3.1.1 and Flask=2.0.1 requirements
    - Integrate Jenkins with GitHub webhook and Jenkins GitHub plugin
    - Run Jenkinsfile from the correct path(usually from master branch)
    - Initiate a private repository in ECR
    - Log in from EC2 agent to ECR with aws get-login-password | docker login to <user-aws-id>.dkr.ecr.<region>.amazonaws.com
    - Tag the image to <user-aws-id>.dkr.ecr.<region>.amazonaws.com/<repo>:<tag> and push it. See ECR documentation for more information.
    - Create an EKS cluster with Node group and assign IAM roles for each entity. The roles for nodes should include AmazonEKSWorkerNodePolicy and AmazonEC2ContainerRegistryReadOnly. See the EKS documentation for more information.
    - Integrate Agents and download kubectl and aws-cli to connect EC2 to EKS
    - Update kubeconfig
    - (OPTIONAL) Apply deployment.yaml and service.yaml to deploy the application to EKS from ECR as a Deployment. See the EKS documentation for more information.
    - Apply podspec.yaml and podservice.yaml to deploy the application to EKS from ECR as a Pod.
        

- Deployment

To deploy the pod or deployment to your EKS cluster, simply run the following command:

    kubectl apply -f podspec.yaml
    kubectl apply -f podservice.yaml

This will create the necessary pod and service resources in your EKS cluster. You can customize the podspec.yaml and podservice.yaml files to fit your own needs.


- Testing LoadBalancer

Once the pod or deployment and service resources are created, you can test the LoadBalancer functionality by running the following command:

    kubectl get services

This will output a list of services in your EKS cluster. You can then open a web browser and navigate to http://<LoadBalancer DNS name>:80 to see the
