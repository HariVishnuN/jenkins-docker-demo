pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t my-node-app:${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Test Container') {
            steps {
                script {
                    sh "docker run -d --name test-app -p 3001:3000 my-node-app:${env.BUILD_NUMBER}"
                    sleep 3
                    sh "curl http://localhost:3001"
                    sh "docker stop test-app && docker rm test-app"
                }
            }
        }
    }
}