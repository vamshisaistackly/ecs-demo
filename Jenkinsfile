pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ecs-demo .'
            }
        }

        stage('AWS Operations') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'aws-credentials'
                ]]) {

                    sh '''
                    aws ecr get-login-password --region us-east-1 | \
                    docker login --username AWS --password-stdin 412664885682.dkr.ecr.us-east-1.amazonaws.com

                    docker tag ecs-demo:latest 412664885682.dkr.ecr.us-east-1.amazonaws.com/ecs-demo:latest

                    docker push 412664885682.dkr.ecr.us-east-1.amazonaws.com/ecs-demo:latest

                    aws ecs update-service \
                    --cluster ecs-demo-cluster \
                    --service ecs-service \
                    --force-new-deployment
                    '''
                }
            }
        }
    }
}