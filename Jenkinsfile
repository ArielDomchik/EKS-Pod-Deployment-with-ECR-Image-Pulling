pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        dir('/home/ubuntu/workspace/ECR+EKS/src') {
          sh 'sudo docker build -t pythonapp'
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
        sh 'echo this step is for ECR push'
      }
    }
  }
}
