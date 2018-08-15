pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('tr-build_aviatrix') {
          steps {
            build 'tr-build_aviatrix'
          }
        }
        stage('tr-account') {
          steps {
            build 'tr-account'
          }
        }
      }
    }
    stage('Gateway') {
      steps {
        build 'tr-gateway'
      }
    }
    stage('Report') {
      steps {
        addShortText 'Done Terraform Regression'
      }
    }
  }
}