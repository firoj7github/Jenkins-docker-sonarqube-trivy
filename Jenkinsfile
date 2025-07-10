pipeline {
    agent any

    environment {
        IMAGE_NAME = 'laravel-jenkins'                      // Corrected variable name
        DOCKER_CREDENTIALS_ID = 'dockerhub'                 // Jenkins credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                // Git repo থেকে কোড clone/pull করে
                checkout scm
            }
        }

        stage('Install Composer Dependencies') {
            steps {
                sh 'composer install --no-dev --optimize-autoloader'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Docker image build করে
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS_ID}") {
                        dockerImage.push()
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Docker image build & pushed successfully!"
        }

        failure {
            echo "❌ Build failed!"
        }
    }
}
