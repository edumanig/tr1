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
        stage('') {
          steps {
            build 'tr-gateway'
          }
        }
      }
    }
  }
}