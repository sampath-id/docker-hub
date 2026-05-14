pipeline {
    agent any

    environment {

        AWS_ACCOUNT_ID = "236726878226"
        AWS_REGION     = "ap-south-1"
        ECR_REPO       = "app"

        LOCAL_IMAGE = "my-app:latest"

        ECR_IMAGE = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:latest"
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
                sh "docker build -t ${LOCAL_IMAGE} ."
            }
        }

        stage('Configure AWS Credentials') {
            steps {

                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {

                    sh '''
                    aws sts get-caller-identity
                    '''
                }
            }
        }

        stage('Login to Amazon ECR') {
            steps {

                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-creds'
                ]]) {

                    sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin \
                    $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                    '''
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh "docker tag ${LOCAL_IMAGE} ${ECR_IMAGE}"
            }
        }

        stage('Push Image to ECR') {
            steps {
                sh "docker push ${ECR_IMAGE}"
            }
        }

    }

    post {

        success {
            echo 'Docker image pushed to Amazon ECR successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
