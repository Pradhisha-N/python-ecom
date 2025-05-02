pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git url:'https://github.com/Pradhisha-N/python-ecom.git',branch:'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t pradhisha/python-webapp:latest .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    sh 'docker push pradhisha/python-webapp:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
