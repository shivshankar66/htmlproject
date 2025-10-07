pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-login')
        DOCKER_IMAGE = 'shivam193/htmlproj:latest'

    }
    stages {
        stage('checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build docker image') {
            steps {
                bat """ 
                docker build -t %DOCKER_IMAGE% .
                """
            }
        }
        stage('login and push Docker Hub') {
            steps {
                bat """
                echo %DOCKER_CREDENTIALS_PSW% | docker login -u %DOCKER_CREDENTIALS_USR% --password-stdin
                docker push %DOCKER_IMAGE%
                """
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat """
                kubectl apply -f deployment.yaml
                """
            }
        }

    
    }
}