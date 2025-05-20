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

        stage('Verify Docker') {
            steps {
                // Check if Docker is installed and running
                sh 'docker --version'  // To verify Docker is installed
                sh 'docker info'       // To get more details about Docker
            }
        }

        stage('Clean Docker') {
            steps {
                // Remove unused Docker images (only if Docker is working)
                sh 'docker system prune -af || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Print the current directory for debugging purposes
                sh 'pwd'
                sh 'ls -l'  // To list all files and confirm Dockerfile's location
                
                // Adjust the path if necessary (change it if Dockerfile is in a subdir)
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
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
                sh "docker push ${DOCKER_IMAGE}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
