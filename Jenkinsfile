pipeline {
    agent any
    environment {
        BASE_URL = "https://fakerestapi.azurewebsites.net"
        IMAGE_NAME = "bookstore-api-js-tests"
    }
    stages {
        stage('Checkout') {
            steps {
                // Let Jenkins handle the checkout in the workspace
                git branch: 'main', url: 'https://github.com/Nebojsha999/bookstore-api-js.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image $IMAGE_NAME..."
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage('Run Tests in Container') {
            steps {
                script {
                    echo "Running tests in Docker container..."
                    sh """
                        mkdir -p $WORKSPACE/reports
                        docker run --rm \
                            -e BASE_URL=$BASE_URL \
                            -v $WORKSPACE/reports:/app/reports \
                            $IMAGE_NAME
                    """
                }
            }
            post {
                always {
                    echo "Archiving test reports..."
                    archiveArtifacts artifacts: 'reports/**', fingerprint: true
                    junit allowEmptyResults: true, testResults: 'reports/*.xml'
                }
            }
        }
    }
}
