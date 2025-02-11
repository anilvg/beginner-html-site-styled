pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t anilvg/website:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: '46f234f6-70a8-4e6f-9b62-6723df022e2d', url: '']) {
                    sh 'docker push anilvg/website:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
