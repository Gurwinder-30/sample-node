pipeline {
    agent any 

    environment {
        DOCKER_IMAGE = 'my-node-app' // Name of the Docker image
        PORT = '9001' // Port to run the application
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the GitHub repository
                git url: 'https://github.com/username/repository-name.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove existing container if it exists
                    sh '''
                    if [ "$(docker ps -q -f name=${DOCKER_IMAGE})" ]; then
                        docker stop ${DOCKER_IMAGE}
                        docker rm ${DOCKER_IMAGE}
                    fi
                    '''
                    // Run the Docker container
                    sh "docker run -d -p ${PORT}:${PORT} --name ${DOCKER_IMAGE} ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            // Clean up the workspace
            cleanWs()
        }
    }
}
