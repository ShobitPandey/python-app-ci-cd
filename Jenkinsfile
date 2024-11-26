dpipeline {
    agent any

    environment {
        // Docker image name
        DOCKER_IMAGE = 'shobitpandey18/python-app-image'
        
        DOCKER_TAG = "${env.BUILD_NUMBER}"
        // Docker credentials
        DOCKER_USERNAME = 'shobitpandey18'   
        DOCKER_PASSWORD = 'Pandey$docker18'  
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
                    // Log in to Docker Hub using the provided environment variables
                    echo "Logging in to Docker Hub..."
                    sh """
                        echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                    """
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the built image to Docker Hub with the tag
                    echo "Pushing image ${DOCKER_IMAGE}:${DOCKER_TAG} to Docker Hub..."
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                }
            }
        }

        stage('Check Docker Installation') {
            steps {
                // Verify that Docker is available in the Jenkins environment
                sh 'docker --version'
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
