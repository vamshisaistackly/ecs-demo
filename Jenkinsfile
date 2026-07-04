pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'ecs-demo'
        ACCOUNT_ID = '412664885682'
    }

    stages {

    stage('Checkout') {
    steps {
        git branch: 'main',
        url: 'https://github.com/vamshisaistackly/ecs-demo.git'
    }
}

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ecs-demo .'
            }
        }

        stage('Login ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag ecs-demo:latest \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/ecs-demo:latest
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/ecs-demo:latest
                '''
            }
        }

        stage('Force ECS Deployment') {
            steps {
                sh '''
                aws ecs update-service \
                --cluster ecs-demo-cluster \
                --service ecs-demo-task-service-90sgcgdp \
                --force-new-deployment
                '''
            }
        }
    }
}