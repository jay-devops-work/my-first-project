pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jaydevopswork/my-nginx-app:latest -t jaydevopswork/my-nginx-app:v%BUILD_NUMBER% .'
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
                bat 'docker push jaydevopswork/my-nginx-app:latest'
                bat 'docker push jaydevopswork/my-nginx-app:v%BUILD_NUMBER%'
            }
        }

        stage('Deploy to eC2') {
            steps {
                bat '''
                ssh -o StrictHostKeyChecking=no -i my-nginx-project-1-key.pem ubuntu@ec2-54-173-118-242.compute-1.amazonaws.com "docker pull jaydevopswork/my-nginx-app:latest && docker stop my-nginx-app || true && docker rm my-nginx-app || true && docker run -d -p 80:80 --name my-nginx-app jaydevopswork/my-nginx-app:latest"
                '''
            }
        }
    }
}