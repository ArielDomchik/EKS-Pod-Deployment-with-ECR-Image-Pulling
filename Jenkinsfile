pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        dir('/home/ubuntu/workspace/ECR+EKS/src') {
          sh 'sudo docker build -t pythonapp .'
          sh 'sudo docker run -d -p 8080:8080 --name pythonapp pythonapp'
        }
      }
    }
    stage('Test') {
      steps {
        sh 'echo test'
      }
    }
    stage('Clean') {
      steps {
        sh 'sudo docker stop pythonapp'
        sh 'sudo docker rm pythonapp'
      }
    }
    stage('Push to ECR') {
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin public.ecr.aws'
	sh 'sudo docker tag pythonapp public.ecr.aws/x3n7f5y0/arieldomchik:pythonapp'
	sh 'sudo docker push public.ecr.aws/x3n7f5y0/arieldomchik:pythonapp'
      }
    }
    stage('Deploy to EKS') {
	agent { label 'Slave 2' }
	  steps {
		sh 'kubectl apply -f deployment.yaml'
		sh 'kubectl apply -f service.yaml'
		#sh 'kubectl expose deployment -n leumi eks-sample-linux-deployment --type=LoadBalancer --name=service'
		sh 'kubectl describe svc -n leumi service'
  } 
}
}
}
