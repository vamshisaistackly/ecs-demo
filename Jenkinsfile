pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '412664885682'
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'ecs-demo'
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ecs-demo .'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-1 | \
                docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag ecs-demo:latest \
                ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:latest
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push \
                ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:latest
                '''
            }
        }
    }
}