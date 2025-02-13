pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t anilvg/beginner-html-site-styled:latest .'
      }
    }
    stage('Push Docker Image') {
      steps {
        sh 'docker push anilvg/beginner-html-site-styled:latest'
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }
}
