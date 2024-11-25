pipeline {
    agent any
    
    environment {
        // Replace with your Docker Hub username and image name
        DOCKER_IMAGE = 'shobitpandey18/python-app-image'  
        DOCKER_USERNAME = 'shobitpandey18'    
        DOCKER_PASSWORD = 'Pandey$docker18'    
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository containing your app
                git 'https://github.com/ShobitPandey/python-app-ci-cd.git',branch: 'main'
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
                    // Log in to Docker Hub using the credentials provided in the environment variables
                    sh """
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    """
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
