pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('tr-build_aviatrix') {
          steps {
            addHtmlBadge 'Terraform Regression 3.4'
            build(propagate: true, job: 'tr-build_aviatrix', quietPeriod: 1, wait: true)
          }
        }
        stage('pylint-check') {
          steps {
            build 'pylint-check'
          }
        }
        stage('tr-upgrade') {
          steps {
            build(job: 'upgrade_only', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Gateway') {
      parallel {
        stage('tr-account') {
          steps {
            build(job: 'tr-account', propagate: true, wait: true)
            build 'tr-gateway_vpn'
          }
        }
        stage('tr-gateway_vpn') {
          steps {
            build(job: 'tr-gateway_vpn', propagate: true, wait: true)
          }
        }
        stage('tr-gateway') {
          steps {
            build(job: 'tr-gateway', propagate: true, wait: true)
          }
        }
        stage('tr-gateway-nat') {
          steps {
            build(job: 'tr-gateway-nat', propagate: true, wait: true)
          }
        }
        stage('tr-gateway-vpn-nat') {
          steps {
            build(job: 'tr-gateway-vpn-nat', propagate: true, wait: true)
          }
        }
        stage('tr-vpn-ldap-duo') {
          steps {
            build(job: 'tr-gateway-ldap-duo', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Report') {
      parallel {
        stage('Report') {
          steps {
            addShortText 'Done Terraform Regression'
          }
        }
        stage('send email') {
          steps {
            mail(subject: 'Terraform Pipeline', body: 'Aviatrix Gateway Creation', to: 'edsel@aviatrix.com', from: 'localhost@aviatrix.com')
          }
        }
      }
    }
    stage('Results') {
      steps {
        junit 'tr1-results'
      }
    }
  }
}