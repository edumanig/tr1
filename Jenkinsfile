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
        stage('pylint-check') {
          steps {
            build 'pylint-check'
          }
        }
      }
    }
    stage('Gateway') {
      parallel {
        stage('tr-account') {
          steps {
            build 'tr-account'
            build 'tr-gateway_vpn'
          }
        }
        stage('tr-gateway_vpn') {
          steps {
            build 'tr-gateway_vpn'
          }
        }
        stage('tr-gateway') {
          steps {
            build 'tr-gateway'
          }
        }
        stage('tr-gateway-nat') {
          steps {
            build 'tr-gateway-nat'
          }
        }
        stage('tr-gateway-vpn-nat') {
          steps {
            build 'tr-gateway-vpn-nat'
          }
        }
        stage('tr-vpn-ldap-duo') {
          steps {
            build 'tr-gateway-ldap-duo'
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