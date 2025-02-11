pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = '46f234f6-70a8-4e6f-9b62-6723df022e2d'
        KUBECONFIG_CREDENTIALS_ID = 'kubeconfig-jenkins'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'gh-pages', url: 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t anilvg/website:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: DOCKER_CREDENTIALS_ID]) {
                    sh 'docker push anilvg/website:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: KUBECONFIG_CREDENTIALS_ID]) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
    }
}
