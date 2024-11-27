pipeline {
    agent any

    environment {
    
        DOCKER_IMAGE = 'shobitpandey18/python-app-image'
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
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Log in to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Log in to Docker Hub using the credentials provided
                        sh """
                           echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        """
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the built image to Docker Hub
                    docker.image(DOCKER_IMAGE).push()
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

