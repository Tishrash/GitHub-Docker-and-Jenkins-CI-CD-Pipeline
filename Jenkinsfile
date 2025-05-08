pipeline {
    agent any
    
    stages {
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Tishrash/GitHub-Docker-and-Jenkins-CI-CD-Pipeline.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t rashmikagamage/nodeapp-cuban:${BUILD_NUMBER} .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-docker', variable: 'test-docker')]) {
                    script {  
                        sh "docker login -u rashmikagamage -p '${test-docker}'"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                sh "docker push rashmikagamage/nodeapp-cuban:${BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
