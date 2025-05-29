pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/bindukantharaju/resubmissionrepo-banking-deployment12.git', branch: 'master'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t bindukantharaju/banking:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push bindukantharaju/banking:v1'
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'sudo docker run -itd --name banking-container -p 8083:8081 bindukantharaju/banking:v1'
            }
        }
    }
}
