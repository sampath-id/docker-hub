
  pipeline {
    agent any

    environment {
        LOCAL_IMAGE = "my-app:latest"
        DOCKERHUB_IMAGE = "sampathid/my-app:latest"
    }

    stages {

        stage('Tag Image') {
            steps {
                sh 'docker tag $LOCAL_IMAGE $DOCKERHUB_IMAGE'
            }
        }

        stage('Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: '8050050150',
                    passwordVariable: 'S@Mp@th502'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                sh 'docker push $DOCKERHUB_IMAGE'
            }
        }
    }
}          
