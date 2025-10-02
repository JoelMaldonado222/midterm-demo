pipeline {
  agent any

  tools {
    jdk 'jdk17'        // must match the name you set in Jenkins Tools
    maven 'maven3'     // must match the name you set in Jenkins Tools
  }

  environment {
    DOCKER_IMAGE = 'midterm-demo:ci'
  }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn -B -ntp clean verify'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
        }
      }
    }

    stage('Docker Build') {
      when { expression { fileExists('Dockerfile') } }
      steps {
        sh 'docker build -t ${DOCKER_IMAGE} .'
      }
    }

    stage('Run Container (Smoke)') {
      when { expression { fileExists('Dockerfile') } }
      steps {
        sh 'docker rm -f midterm-demo || true'
        sh 'docker run -d --name midterm-demo -p 8080:8080 ${DOCKER_IMAGE}'
        sh 'sleep 5'
        sh 'curl -f http://localhost:8080/hello'
      }
      post {
        always {
          sh 'docker logs midterm-demo || true'
          sh 'docker rm -f midterm-demo || true'
        }
      }
    }
  }
}
