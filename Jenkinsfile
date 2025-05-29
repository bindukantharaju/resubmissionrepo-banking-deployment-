pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = "bindukantharaju/bindubanking"
        IMAGE_TAG = "v1"
        IMAGE_NAME = "${DOCKER_HUB_REPO}:${IMAGE_TAG}"
        CONTAINER_NAME = "bindubanking123"
        HOST_PORT = "8083"
        CONTAINER_PORT = "8081"
        REPO_URL = "https://github.com/bindukantharaju/resubmissionrepo-banking-deployment12.git"
        BRANCH = "master"
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
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Stop & Remove Existing Container') {
            steps {
                script {
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
                sh "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}"
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


