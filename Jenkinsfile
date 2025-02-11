pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "anilvg/website:latest"
        DOCKER_CREDENTIALS_ID = "46f234f6-70a8-4e6f-9b62-6723df022e2d"
        KUBECONFIG_CREDENTIALS_ID = "kubeconfig-jenkins"
    }

    options {
        skipDefaultCheckout()
        disableConcurrentBuilds()
        preserveStashes()
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'gh-pages', 
                    credentialsId: 'anilvg', 
                    url: 'https://github.com/anilvg/beginner-html-site-styled.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (!fileExists('Dockerfile')) {
                        error("Dockerfile not found in repository. Please add a valid Dockerfile.")
                    }
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 46f234f6-70a8-4e6f-9b62-6723df022e2d , url: ""]) {
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([file(credentialsId: kubeconfig-jenkins, variable: 'KUBECONFIG')]) {
                        sh 'kubectl apply -f deployment.yaml'
                        sh 'kubectl apply -f service.yaml'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Access the website at http://3.16.28.96:30010"
        }
        failure {
            echo "Pipeline failed! Check logs for more details."
        }
    }
}
