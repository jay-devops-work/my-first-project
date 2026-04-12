pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jaydevopswork/my-nginx-app:v1 .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push jaydevopswork/my-nginx-app:v1'
            }
        }

        stage('Deploy Container') {
            steps {
                bat '''
                docker stop my-nginx-app 2>nul
                docker rm my-nginx-app 2>nul
                docker run -d -p 8081:80 --name my-nginx-app jaydevopswork/my-nginx-app:v1
                '''
            }
        }

        stage('Test') {
            steps {
                bat 'curl http://localhost:8081'
            }
        }
    }
}