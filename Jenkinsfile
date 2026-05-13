
pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-app:latest"
        IMAGE_TAG  = "v1"
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/sampath-id/docker-hub.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: '8050050150',
                    passwordVariable: 'S@mp@th502'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
