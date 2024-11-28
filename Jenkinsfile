pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1' // Replace with your AWS region
        ECR_REPO1 = '<aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/service1'
        ECR_REPO2 = '<aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/service2'
        ECR_REPO3 = '<aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/service3'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml' // Path to your docker-compose file
    }

        stage('Authenticate with AWS ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 575108934554.dkr.ecr.us-east-1.amazonaws.com
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                docker-compose -f $DOCKER_COMPOSE_FILE build
                '''
            }
        }

        stage('Tag and Push Docker Images') {
            steps {
                sh '''
                docker tag 3tier-nodejs-frontend:latest 575108934554.dkr.ecr.us-east-1.amazonaws.com/my-repository:latest
                docker tag 3tier-nodejs-backend:latest 575108934554.dkr.ecr.us-east-1.amazonaws.com/my-repository:latest
                docker tag mongo:latest 575108934554.dkr.ecr.us-east-1.amazonaws.com/my-repository:latest

                # Push the images to AWS ECR
                docker push 575108934554.dkr.ecr.us-east-1.amazonaws.com/my-repository:latest
                docker push $ECR_REPO2:latest
                docker push $ECR_REPO3:latest
                '''
            }
        }

        stage('Apply Kubernetes Manifests') {
            steps {
                sh '''
                # Use kubectl to apply Kubernetes manifests
                kubectl apply -f kubernetes/deployments/
                '''
            }
        }

        stage('Apply Network Policies') {
            steps {
                sh '''
                # Apply network policies for security best practices
                kubectl apply -f kubernetes/network-policies/
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
