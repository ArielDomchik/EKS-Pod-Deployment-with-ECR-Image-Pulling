
## EKS Pod Deployment with ECR Image Pulling

This project provides a simple way to deploy a pod with a service to an EKS cluster, which pulls images from an ECR repository in AWS.
The image being pulled from the ECR is a simple web application that shows the hostname and IP of the host. 

### Prerequisites

Before you can deploy the pod to your EKS cluster, you'll need the following:

-   A running EKS cluster
-   Access to an ECR repository in your AWS account
-   Jenkins installed with a master and two agent servers
-   `kubectl` command-line tool installed

### Setup

Follow these steps to set up this project:

1.  Create a GitLab user and repository for the project.
2.  Import a public SSH key to the GitHub project to allow accessibility.
3.  Upload the project to the GitLab repository.
4.  Integrate Jenkins with GitHub webhook and Jenkins GitHub plugin.
5.  Run `Jenkinsfile` from the correct path (usually from the master branch).
6.  Initiate a private repository in ECR.
7.  Log in from EC2 agent to ECR with `aws get-login-password | docker login` to `<user-aws-id>.dkr.ecr.<region>.amazonaws.com`.
8.  Tag the image to `<user-aws-id>.dkr.ecr.<region>.amazonaws.com/<repo>:<tag>` and push it. See ECR documentation for more information.
9.  Create an EKS cluster with a Node group and assign IAM roles for each entity. The roles for nodes should include `AmazonEKSWorkerNodePolicy` and `AmazonEC2ContainerRegistryReadOnly`. See the EKS documentation for more information.
10.  Integrate agents and download `kubectl` and `aws-cli` to connect EC2 to EKS.
11.  Update `kubeconfig`.
12.  Customize `Jenkinsfile` for your own purposes, with appropiate customization to `podspec.yaml` and `podservice.yaml` too.
13.  Run the pipeline and enjoy the deployment of your pod in eks.

This will create the necessary pod and service resources in your EKS cluster. You can customize the `podspec.yaml` and `podservice.yaml` files to fit your own needs.

### Testing Load Balancer

Once the pod and service resources are created, you can test the LoadBalancer functionality by running the following command:

`kubectl get services` 

This will output a list of services in your EKS cluster. You can then open a web browser and navigate to `https://<load_balancer_dns_name>:443` to see the load balancer in action.
