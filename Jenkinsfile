pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'gh-pages', 
                    url: 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'ls -la'  // Debug: Check if Dockerfile is present
                sh 'docker build -t anilvg/website:latest .'
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
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
