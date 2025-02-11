pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'ls -la'  // Debugging step
                sh 'docker build -t anilvg/website:latest .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'
                sh 'docker push anilvg/website:latest'
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
