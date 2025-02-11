pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'anilvg/website:latest'
        K8S_DEPLOYMENT = 'website-deployment'
        K8S_NAMESPACE = 'default'
        GIT_REPO = 'https://github.com/anilvg/beginner-html-site-styled.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'gh-pages', url: env.GIT_REPO
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: '46f234f6-70a8-4e6f-9b62-6723df022e2d', url: '']) {
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    kubectl set image deployment/$K8S_DEPLOYMENT website=$DOCKER_IMAGE --namespace=$K8S_NAMESPACE
                    kubectl rollout status deployment/$K8S_DEPLOYMENT --namespace=$K8S_NAMESPACE
                    '''
                }
            }
        }
    }
}
