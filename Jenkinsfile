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
                sh 'docker build -t simpleWebapp .'
            }
        }

        stage('Push') {
            steps {
                script {
                    sh 'docker push khatab24/simpleWebapp'
                    sh 'docker rmi khatab24/simpleWebapp'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh 'docker pull khatab24/simpleWebapp'
                    sh 'docker run -d --name mywebsite -p 5000:80 khatab24/simpleWebapp'
                }
            }
        }
    }
}


