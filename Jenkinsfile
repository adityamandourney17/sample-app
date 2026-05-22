pipeline {
    agent any

    environment {
        IMAGE_NAME = "adityamandourney/sample-app"
    }

    stages {

        stage('Checkout') {
            steps {
                 git branch: 'main', url: 'https://github.com/adityamandourney17/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'

                    sh 'docker push $IMAGE_NAME'
                }
            }
        }

        stage('Deploy') {
            steps {

                sh '''
                docker stop sample-app || true
                docker rm sample-app || true

                docker run -d \
                --name sample-app \
                -p 80:3000 \
                $IMAGE_NAME
                '''
            }
        }
    }
}
