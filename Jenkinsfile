pipeline {
    agent any

    stages {

        stage('Verify Docker') {
            steps {
                sh 'docker --version'
                sh 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-flask-app:latest .'
            }
        }

        stage('Verify Image') {
            steps {
                sh 'docker images devops-flask-app'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f flask-container || true

                    docker run -d \
                        --name flask-container \
                        --restart unless-stopped \
                        -p 5000:5000 \
                        devops-flask-app:latest
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    for i in 1 2 3 4 5; do
                        if curl -fsS http://localhost:5000/; then
                            echo "Application is running successfully!"
                            exit 0
                        fi
                        sleep 2
                    done

                    echo "Application health check failed"
                    exit 1
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Jenkins console output.'
        }
    }
}
