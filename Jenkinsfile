pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('tr-build_aviatrix') {
          steps {
            build 'tr-build_aviatrix'
            addHtmlBadge 'Terraform Regression 3.4'
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
      parallel {
        stage('tr-gateway') {
          steps {
            build 'tr-gateway'
          }
        }
        stage('tr-gateway_vpn') {
          steps {
            build 'tr-gateway_vpn'
          }
        }
      }
    }
    stage('Report') {
      steps {
        addShortText 'Done Terraform Regression'
      }
    }
  }
}