
pipeline {
    agent any

    stages {
        stage('Verify Docker') {
            steps {
                sh 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-flask-app .'
            }
        }

        stage('Verify Image') {
            steps {
                sh 'docker images devops-flask-app'
            }
        }
    }
}
