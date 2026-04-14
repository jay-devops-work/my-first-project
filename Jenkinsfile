pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jaydevopswork/my-nginx-app:latest -t jaydevopswork/my-nginx-app:v%BUILD_NUMBER% .'
            }
        }

        stage('login to DockerHub') {
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

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    bat '''
                    ssh -o StrictHostKeyChecking=no ubuntu@ec2-3-88-167-89.compute-1.amazonaws.com "
                    docker pull jaydevopswork/my-nginx-app:latest &&
                    docker stop my-nginx-app || true &&
                    docker rm my-nginx-app || true &&
                    docker run -d -p 80:80 --name my-nginx-app jaydevopswork/my-nginx-app:latest
                    "
                    '''
                }
            }
        }
    }
}