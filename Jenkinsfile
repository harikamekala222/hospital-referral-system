pipeline {

    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Stop Old Containers') {
            steps {
                sh '''
                    docker compose down || true
                '''
            }
        }

        stage('Build Docker Images') {
            steps {
                sh '''
                    docker compose build --no-cache
                '''
            }
        }

        stage('Start Application') {
            steps {
                sh '''
                    docker compose up -d
                '''
            }
        }

        stage('Check Containers') {
            steps {
                sh '''
                    docker compose ps
                '''
            }
        }
    }

    post {

        success {
            echo 'Hospital Referral System deployed successfully!'
        }

        failure {
            echo 'Deployment failed. Check Jenkins console logs.'
        }

        always {
            sh '''
                docker compose ps || true
            '''
        }
    }
}
