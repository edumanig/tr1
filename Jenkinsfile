pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build') {
          steps {
            build 'tr-account'
          }
        }
        stage('error') {
          steps {
            build 'tr-gateway'
          }
        }
      }
    }
  }
}