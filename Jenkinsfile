pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t devops-flask-app .'
            }
        }

        stage('Verify Image') {
            steps {
                bat 'docker images devops-flask-app'
            }
        }
    }
}
