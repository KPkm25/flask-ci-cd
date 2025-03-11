pipeline {
    agent {
        kubernetes {
            label 'k8s-agent'
            defaultContainer 'jnlp'
        }
    }
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-creds')
    }
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/kpkm25/flask-ci-cd.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                container('jnlp') {
                    sh 'docker build -t kpkm25/flask-ci-cd:latest .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                container('jnlp') {
                    withDockerRegistry([credentialsId: 'docker-creds', url: '']) {
                        sh 'docker push kpkm25/flask-ci-cd:latest'
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                
            }
        }
    }
}
