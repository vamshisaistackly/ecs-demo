pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ecs-demo .'
            }
        }

        stage('Login ECR') {
    steps {
        withCredentials([[
            $class: 'AmazonWebServicesCredentialsBinding',
            credentialsId: 'aws-credentials'
        ]]) {
            sh '''
            aws ecr get-login-password --region us-east-1 | \
            docker login --username AWS --password-stdin 412664885682.dkr.ecr.us-east-1.amazonaws.com
            '''
        }
    }
}

        stage('Tag Image') {
            steps {
                sh 'docker tag ecs-demo:latest 412664885682.dkr.ecr.us-east-1.amazonaws.com/ecs-demo:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push 412664885682.dkr.ecr.us-east-1.amazonaws.com/ecs-demo:latest'
            }
        }

        stage('Force ECS Deployment') {
            steps {
                sh '''
                aws ecs update-service \
                --cluster my-first-cluster \
                --service ecs-demo-service \
                --force-new-deployment
                '''
            }
        }
    }
}