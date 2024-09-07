pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('DockerHub Login') {
            steps {
                script {
                    sh '''
                        echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t simplewebapp .'
            }
        }

        stage('Push') {
            steps {
                script {
                    sh 'docker push khatab24/simplewebapp'
                    sh 'docker rmi khatab24/simplewebapp'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker pull khatab24/simplewebapp'
                    sh 'docker run -d --name mywebsite -p 5000:80 khatab24/simplewebapp'
                }
            }
        }
    }
}


