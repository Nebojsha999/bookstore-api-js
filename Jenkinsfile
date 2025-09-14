pipeline {
  agent any
  environment {
    BASE_URL = "https://fakerestapi.azurewebsites.net"
    IMAGE_NAME = "bookstore-api-js-tests"
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }
    stage('Run Tests in Container') {
      steps {
        sh 'docker run --rm -e BASE_URL=$BASE_URL -v $WORKSPACE/reports:/app/reports $IMAGE_NAME'
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
