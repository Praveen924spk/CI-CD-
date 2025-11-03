pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "906634422086.dkr.ecr.us-east-1.amazonaws.com/myapp"
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Praveen924spk/CI-CD-.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Login to ECR') {
            steps {
                sh """
                aws ecr get-login-password --region ${AWS_REGION} \
                | docker login --username AWS --password-stdin ${ECR_REPO}
                """
            }
        }

        stage('Tag Image') {
            steps {
                sh """
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REPO}:${IMAGE_TAG}
                """
            }
        }

        stage('Push to ECR') {
            steps {
                sh """
                docker push ${ECR_REPO}:${IMAGE_TAG}
                """
            }
        }
    }
}
