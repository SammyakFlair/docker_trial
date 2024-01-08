pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_HUB_USERNAME = 'sammyak'
        DOCKER_HUB_PASSWORD = 'Samdeo@123'
        IMAGE_NAME = "sammyak/try-app-img"
        CONTAINER_NAME = "try-app-cont"
    }

    stages {
        stage('Build Image') {
            steps {
                script {
                    // Build Docker image
                    bat "npm i"
                    bat "npm run build" // Corrected command
                    bat "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    bat "echo ${DOCKER_HUB_PASSWORD} | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin"
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    // Push Docker image to Docker Hub
                    bat "docker push ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Stop and Remove Container') {
            steps {
                script {
                    // Stop and remove existing container (if any)
                    bat "docker stop ${CONTAINER_NAME} || true"
                    bat "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run a new container based on the latest image
                    bat "docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}
