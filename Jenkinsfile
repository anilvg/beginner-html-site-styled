pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t anilvg/website:latest .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'docker login -u anilvg -p cipahane_V5'
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
