pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'Git-creds',
                    url: 'https://github.com/harikamekala222/hospital-referral-system.git'
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh '''
                    docker compose down --remove-orphans || true

                    docker rm -f hospital-mysql hospital-backend hospital-frontend 2>/dev/null || true
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build --no-cache'
            }
        }

        stage('Create Backend Env') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'hospital-backend-env',
                        variable: 'BACKEND_ENV'
                    )
                ]) {
                    sh '''
                        cp "$BACKEND_ENV" backend/.env
                        chmod 600 backend/.env
                    '''
                }
            }
        }

        stage('Start Application') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Check Containers') {
            steps {
                sh '''
                    docker compose ps
                    docker ps
                '''
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            sh 'docker compose ps || true'
            echo 'Deployment failed. Check Jenkins console logs.'
        }
    }
}
