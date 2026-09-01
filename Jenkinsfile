pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/harikamekala222/hospital-referral-system.git'
            }
        }

        stage('Build Docker Images') {
            steps {
                sh 'docker compose build'
            }
        }

        stage('Create Backend Env') {
            steps {
                withCredentials([
                    file(credentialsId: 'backend.env', variable: 'BACKEND_ENV')
                ]) {
                    sh '''
                        cp "$BACKEND_ENV" backend/.env
                        ls -l backend/.env
                    '''
                }
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                    docker compose down
                    docker compose up -d
                '''
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
            echo 'Deployment successful.'
        }
        failure {
            sh 'docker compose ps || true'
            echo 'Deployment failed. Check Jenkins console logs.'
        }
    }
}
