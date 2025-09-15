pipeline {
    agent any
    environment {
        BASE_URL = "https://fakerestapi.azurewebsites.net"
        IMAGE_NAME = "bookstore-api-js-tests"
    }
    options { skipDefaultCheckout(true) }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Nebojsha999/bookstore-api-js.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Run Tests in Container') {
            steps {
                sh '''
                mkdir -p reports
                docker run --rm -e BASE_URL=$BASE_URL -v $WORKSPACE/reports:/app/reports $IMAGE_NAME
                '''
            }
            post {
                always {
                    archiveArtifacts artifacts: 'reports/**', fingerprint: true
                    junit allowEmptyResults: true, testResults: 'reports/*.xml'
                }
            }
        }
    }
}
