pipeline {
    agent any

    environment {
        IMAGE_NAME = "bindubanking"
        CONTAINER_NAME = "bindubanking123"
        HOST_PORT = "8083"
        CONTAINER_PORT = "8081"
        REPO_URL = "https://github.com/bindukantharaju/resubmissionrepo-banking-deployment12.git"
        BRANCH = "main" // change if needed
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Stop & Remove Existing Container') {
            steps {
                script {
                    // Stop and remove container if exists (ignore errors)
                    sh """
                    if [ \$(docker ps -q -f name=${CONTAINER_NAME}) ]; then
                        docker stop ${CONTAINER_NAME}
                        docker rm ${CONTAINER_NAME}
                    fi
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                    docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}:latest
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

