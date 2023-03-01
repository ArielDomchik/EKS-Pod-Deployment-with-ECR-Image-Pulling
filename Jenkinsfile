pipeline {
  agent none
  stages {
    stage('Build') {
     agent { label 'Slave 1' }
      steps {
        dir('/home/ubuntu/workspace/ECR+EKS/src') {
          sh 'sudo docker build -t pythonapp .'
          sh 'sudo docker run -d -p 8080:8080 --name pythonapp pythonapp'
        }
      }
    }
    stage('Test') {
     agent { label 'Slave 1' }
      steps {
        sh 'echo test'
      }
    }
    stage('Clean') {
     agent { label 'Slave 1' }
      steps {
        sh 'sudo docker stop pythonapp'
        sh 'sudo docker rm pythonapp'
      }
    }
    stage('Push to ECR') {
     agent { label 'Slave 1' }
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws'
	sh 'sudo docker tag pythonapp public.ecr.aws/x3n7f5y0/arieldomchik:pythonapp'
	sh 'sudo docker push public.ecr.aws/x3n7f5y0/arieldomchik:pythonapp'
      }
    }
    stage('Deploy to EKS') {
	agent { label 'Slave 2' }
	  steps {
		dir('/home/ubuntu/workspace/ECR+EKS/k8s-configuration/') {
		sh 'kubectl apply -f podspec.yaml'
		sh 'kubectl apply -f podservice.yaml'		
		sh 'kubectl describe svc -n leumi pod-service'
		}
             } 
         }
     }
}
