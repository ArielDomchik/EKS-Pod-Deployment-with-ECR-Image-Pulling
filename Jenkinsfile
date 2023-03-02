pipeline {
  agent none
  environment { ECR_URL = '646360616404.dkr.ecr.us-east-1.amazonaws.com' }
  stages {
    stage('Build') {
     agent { label 'Slave 1' }
      steps {
        dir('/home/ubuntu/workspace/ECR+EKS/src') {
          sh 'sudo docker build -t python-app .'
    }
  }
}
    stage('Push to ECR') {
     agent { label 'Slave 1' }
      steps {
        sh 'aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin ${ECR_URL}'
	sh 'sudo docker tag python-app ${ECR_URL}/leumi-repository:python-app${BUILD_NUMBER}'
	sh 'sudo docker push ${ECR_URL}/leumi-repository:python-app${BUILD_NUMBER}'
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
