pipeline {
  agent none
  environment { ECR_URL = '646360616404.dkr.ecr.us-east-1.amazonaws.com'
		REPO = 'leumi-repository'
		APP = 'python-app' }
  stages {
    stage('Build') {
     agent { label 'Slave 1' }
      steps {
        dir('/home/ubuntu/workspace/ECR+EKS/src') {
          sh 'docker build -t ${APP} .'
    }
  }
}
    stage('Push to ECR') {
     agent { label 'Slave 1' }
      steps {
        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ECR_URL}'
	sh 'docker tag ${APP} ${ECR_URL}/${REPO}:${APP}${BUILD_NUMBER}'
	sh 'docker push ${ECR_URL}/${REPO}:${APP}${BUILD_NUMBER}'
  }
}
    stage('Deploy to EKS') {
	agent { label 'Slave 2' }
	  steps {
		dir('/home/ubuntu/workspace/ECR+EKS/k8s-configuration/') {
		sh 'kubectl apply -f podspec.yaml'
		sh "kubectl patch pod -n leumi pod-sample --type=json -p '[{\"op\": \"replace\", \"path\": \"/spec/containers/0/image\", \"value\":\"${ECR_URL}/leumi-repository:python-app${BUILD_NUMBER}\"}]'"
		sh 'kubectl apply -f podservice.yaml'	
		sh 'kubectl describe svc -n leumi pod-service'
	}
      } 
    }
  }
}
