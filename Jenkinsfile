pipeline {
    agent any
    environment {
        IMAGE_NAME = 'sampath-id/my-app'  // ✅ dockerhub-username/image
        IMAGE_TAG  = 'latest'
    }
    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sampath-id/docker-hub.git',
                    credentialsId: 'sampath-id'
            }
        }
        stage('Build Docker Image') {
            steps {
                // ✅ Correct tag format
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }
        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: '8050050150',
                    passwordVariable: 'S@mp@th502'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }
}
