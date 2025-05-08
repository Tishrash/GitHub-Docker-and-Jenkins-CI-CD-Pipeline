pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/HGSChandeepa/test-node'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t rashmikagamage/nodeapp-cuban:$BUILD_NUMBER .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-docker', variable: 'DOCKER_PASSWORD')]) {
                    sh '/usr/local/bin/docker login -u rashmikagamage -p $DOCKER_PASSWORD'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '/usr/local/bin/docker push rashmikagamage/nodeapp-cuban:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            sh '/usr/local/bin/docker logout'
        }
    }
}
