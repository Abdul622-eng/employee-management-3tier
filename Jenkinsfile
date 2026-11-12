pipeline {
    agent any

    environment {
        APP_DIR = "/var/lib/jenkins/workspace/backend-server"
    }

    stages {

        stage('Verify Docker') {
            steps {
                sh '''
                docker --version
                docker compose version
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                sh '''
                echo "Building Backend Image..."

                cd $APP_DIR/backend

                docker build -t employee-backend:v4 .
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                echo "Building Frontend Image..."

                cd $APP_DIR/frontend

                docker build -t employee-frontend:v1 .
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                echo "Deploying Application..."

                cd $APP_DIR

                docker compose down

                docker compose up -d --build
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 20

                curl -f http://localhost:8000/health
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment Successful!'
        }

        failure {
            echo 'Deployment Failed!'
        }

        always {
            sh 'docker ps -a'
        }
    }
}
