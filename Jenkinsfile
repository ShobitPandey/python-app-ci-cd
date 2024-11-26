pipeline {
    agent any

    environment {
        // Using Jenkins Credentials for Docker credentials (replace with your credentials ID)
        DOCKER_IMAGE = 'shobitpandey18/python-app-image'
        DOCKER_TAG = "${env.BUILD_NUMBER}"  // Use the build number as the Docker tag
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository containing your app
                git url: 'https://github.com/ShobitPandey/python-app-ci-cd.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile
                    echo "Building Docker image ${DOCKER_IMAGE}:${DOCKER_TAG}..."
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Log in to Docker Hub') {
            steps {
                script {
                    // Log in to Docker Hub using Jenkins credentials store
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        echo "Logging in to Docker Hub..."
                        sh """
                            echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                        """
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the built image to Docker Hub
                    echo "Pushing image to Docker Hub..."
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                }
            }
        }
    }

    post {
        always {
            // Clean the workspace after the pipeline finishes
            cleanWs()
        }
    }
}
