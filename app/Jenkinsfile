pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'yashwanth2003/python_jenkins'  // Updated image name
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Clean Docker') {
            steps {
                // Optional step to clean up unused images
                sh 'docker system prune -af || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image from the correct path
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Verify Docker') {
            steps {
                // Check Docker version for debugging
                sh 'docker --version'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push the image to Docker Hub
                sh "docker push ${DOCKER_IMAGE}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            // Ensure to log out from Docker Hub after each build
            sh 'docker logout'
        }
    }
}
