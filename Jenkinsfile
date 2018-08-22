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
            build(job: 'upgrade_only', propagate: true, wait: true, quietPeriod: 20)
          }
        }
      }
    }
    stage('Gateway') {
      parallel {
        stage('tr-account') {
          steps {
            build(job: 'tr-account', propagate: true, wait: true, quietPeriod: 20)
            build 'tr-gateway_vpn'
          }
        }
        stage('tr-gateway_vpn') {
          steps {
            build(job: 'tr-gateway_vpn', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('tr-gateway') {
          steps {
            build(job: 'tr-gateway', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('tr-gateway-nat') {
          steps {
            build(job: 'tr-gateway-nat', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('tr-gateway-vpn-nat') {
          steps {
            build(job: 'tr-gateway-vpn-nat', propagate: true, wait: true, quietPeriod: 20)
          }
        }
        stage('tr-vpn-ldap-duo') {
          steps {
            build(job: 'tr-gateway-ldap-duo', propagate: true, wait: true, quietPeriod: 20)
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
            mail(subject: 'Terraform Pipeline - 100% Passed', body: 'tr-build_aviatrix, pylint-check, tr-upgrade, tr-account, tr-gateway_vpn, tr-gateway, tr-gateway-nat, tr-gateway-vpn-nat, tr-vpn-ldap-duo', to: 'edsel@aviatrix.com', from: 'localhost@aviatrix.com')
          }
        }
      }
    }
  }
}