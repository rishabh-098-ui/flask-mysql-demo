pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-username')
    DOCKER_IMAGE = "yourdockerhubusername/flask-mysql-demo"
  }
  stages {
    stage('Clone') {
      steps {
        checkout scm
      }
    }
    stage('Build Docker Image') {
      steps {
        script {
          docker.build("$DOCKER_IMAGE")
        }
      }
    }
    stage('Test Container') {
      steps {
        script {
          docker.image("$DOCKER_IMAGE").inside {
            sh 'python app.py & sleep 5'
          }
        }
      }
    }
    stage('Push to Docker Hub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', "${DOCKERHUB_CREDENTIALS}") {
            docker.image("$DOCKER_IMAGE").push()
          }
        }
      }
    }
    stage('Deploy') {
      when {
        expression { return false } // Enable for real deployments
      }
      steps {
        echo 'Deploying image...'
        // Add `docker run ...` command as needed
      }
    }
  }
}