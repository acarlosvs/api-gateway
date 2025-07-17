pipeline {
  agent any

  environment {
    IMAGE_NAME = 'api-gateway/app'
    TAG = 'latest'
  }

  tools {
    maven 'Maven 3.9.6'
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'master', url: 'https://github.com/acarlosvs/api-gateway'
      }
    }

    stage('Build & Test') {
      steps {
        withMaven(maven: 'Maven 3.9.6') {
          sh 'mvn clean verify'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t api-gateway:latest .'
      }
    }

    stage('Deploy') {
      steps {
        sh 'docker stop app || true && docker rm app || true'
        sh 'docker run -d --name app -p 8085:8085 api-gateway:latest'
      }
    }
  }
}
