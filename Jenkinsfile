pipeline {
    agent any
    environment {
        LOCAL_IMAGE      = "my-app:latest"
        DOCKERHUB_IMAGE  = "sampath/my-app:latest"
    }
    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sampath-id/docker-hub.git',
                    credentialsId: 'sampath-id'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${LOCAL_IMAGE} ."  // ✅ double quotes
            }
        }

        stage('Tag Image') {
            steps {
                sh "docker tag ${LOCAL_IMAGE} ${DOCKERHUB_IMAGE}"  // ✅ double quotes
            }
        }

        stage('Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh '''
                        echo "$PASS" | docker login -u "$USER" --password-stdin
                    '''                                // ✅ triple quotes for credentials
                }
            }
        }

        stage('Push') {
            steps {
                sh "docker push ${DOCKERHUB_IMAGE}"   // ✅ double quotes
            }
        }

    }
}
