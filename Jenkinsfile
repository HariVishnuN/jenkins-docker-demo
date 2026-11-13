pipeline {
    agent any
    
    environment {
        IMAGE_NAME = 'my-node-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Test Container') {
            steps {
                script {
                    // Run container temporarily to verify it starts and responds
                    sh "docker run -d --name test-app -p 3001:3000 ${IMAGE_NAME}:latest"
                    sleep 3
                    sh "curl http://localhost:3001"
                    sh "docker stop test-app && docker rm test-app"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully! Docker image built and tested.'
        }
        failure {
            echo 'Pipeline failed during execution.'
        }
    }
}