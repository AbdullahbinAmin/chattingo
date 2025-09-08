pipeline {
    agent any

    environment {
        // These are your Docker Hub credentials
        DOCKERHUB_CREDENTIALS = credentials('Chattingo_Docker_Hub_PAT') // Add your Docker Hub credentials here
        DOCKERHUB_USERNAME = 'abdullahbinamin' // Change this to your username
        REPO_NAME = 'chattingo'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the repository from GitHub...'
            }
        }
        stage('Build and Push Docker Images') {
            steps {
                script {
                    echo 'Building and pushing Docker images to Docker Hub...'

                    // Login to Docker Hub using the credentials you defined in the environment section
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"

                    // Define image names and tags
                    def frontendImage = "${DOCKERHUB_USERNAME}/${REPO_NAME}-frontend:latest"
                    def backendImage = "${DOCKERHUB_USERNAME}/${REPO_NAME}-backend:latest"

                    // Build the backend image
                    sh "docker build -t ${backendImage} ./backend"

                    // Build the frontend image
                    sh "docker build -t ${frontendImage} ./frontend"

                    // Push the backend and frontend images to Docker Hub
                    sh "docker push ${backendImage}"
                    sh "docker push ${frontendImage}"
                }
            }
        }
        stage('Deploy to VPS') {
            steps {
                echo 'Deploying the application to the Hostinger VPS...'
                // We will add the SSH and Docker Compose commands here in the next step.
            }
        }
    }
}