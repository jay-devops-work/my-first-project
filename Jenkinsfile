pipeline{
    agents any
    stages{
        stage('Clone code') {
            steps {
                git 'https://github.com/jay-devops-work/my-first-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jaydevopswork/my-nginx-app:latest .'               
            }
        }
        stage('Push docker Image') {
            steps {
                bat 'docker push jaydevopswork/my-nginx-app:latest'
            }
        }
        stage('Deploy container ') {
            steps {
                bat '''' docker stop my-nginx-app:latest 2>nul
                docker rm my-nginx-app:latest 2>nul
                docker run -d -p 8081:80 jaydevopswork/my-nginx-app:latest
                '''
            }
        }
    }
}