
pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Verify Docker') {
            steps {
                sh 'docker --version'
                sh 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
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

        stage('Deploy to Render') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'render-deploy-hook',
                        variable: 'RENDER_HOOK'
                    )
                ]) {
                    sh '''
                        curl --fail --silent --show-error \
                        -X POST "$RENDER_HOOK"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'All pipeline stages completed successfully!'
            echo 'Render deployment triggered!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
