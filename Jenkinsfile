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
        sh 'aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 646360616404.dkr.ecr.us-east-1.amazonaws.com'
	sh 'sudo docker tag pythonapp 646360616404.dkr.ecr.us-east-1.amazonaws.com/leumi-repository:pythonapp'
	sh 'sudo docker push 646360616404.dkr.ecr.us-east-1.amazonaws.com/leumi-repository:pythonapp'
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
