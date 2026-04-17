pipeline {
    agent any

    environment {
        // Replace with your Docker Hub username and repository name
        DOCKER_REPO = "kennyjaymes/fastapi-docker-project"
        // Replace with your Jenkins Credentials ID for Docker Hub
        DOCKER_HUB_CREDS = "DockerHubCredentialsId"
    }

    stages {
        stage('Diagnostics') {
            steps {
                script {
                    echo "Checking environment..."
                    bat "docker --version"
                    bat "docker info"
                }
            }
        }

        stage('Checkout') {
            steps {
                // Checkout code from source control
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image: ${DOCKER_REPO}:latest"
                    bat "docker build -t ${DOCKER_REPO}:latest ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDS}", 
                                                      usernameVariable: 'DOCKER_USER', 
                                                      passwordVariable: 'DOCKER_PASS')]) {
                        // In Windows batch, we use double quotes for the password and pipe it
                        bat "echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin"
                    }
                }
            }
        }

        stage('Tag and Push Image') {
            steps {
                script {
                    echo "Pushing Docker image: ${DOCKER_REPO}:latest"
                    bat "docker push ${DOCKER_REPO}:latest"
                    
                    // Optional: Tag and push with build number
                    bat "docker tag ${DOCKER_REPO}:latest ${DOCKER_REPO}:%BUILD_NUMBER%"
                    bat "docker push ${DOCKER_REPO}:%BUILD_NUMBER%"
                }
            }
        }
    }

    post {
        always {
            script {
                bat "docker logout"
            }
        }
        success {
            echo "Successfully built and pushed ${DOCKER_REPO}"
        }
        failure {
            echo "Pipeline failed. Check stage logs for details."
        }
    }
}
