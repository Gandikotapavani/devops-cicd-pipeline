pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-flask-app .'
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'docker images devops-flask-app'
            }
        }
    }
}
