pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('tr-account') {
          steps {
            build 'tr-account'
          }
        }
        stage('tr-gateway') {
          steps {
            build 'tr-gateway'
          }
        }
      }
    }
    stage('') {
      steps {
        build 'tr-build_aviatrix'
      }
    }
  }
}