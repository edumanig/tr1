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
    stage('Test1') {
      parallel {
        stage('Gateway Test') {
          steps {
            sleep 5
          }
        }
        stage('tr-account') {
          steps {
            build(job: 'tr-account', propagate: true, wait: true)
          }
        }
        stage('Gateway') {
          steps {
            sleep 10
            build(job: 'tr-gateway', propagate: true, wait: true)
            sleep 20
            build(job: 'tr-gateway-nat', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Test2') {
      parallel {
        stage('VPN Test') {
          steps {
            addShortText 'VPN Gateways'
          }
        }
        stage('Gateway VPN') {
          steps {
            build(job: 'tr-gateway_vpn', propagate: true, wait: true)
            sleep 20
            build(job: 'tr-gateway-vpn-nat', propagate: true, wait: true)
            sleep 20
            build(job: 'tr-gateway-ldap-duo', propagate: true, wait: true)
          }
        }
      }
    }
    stage('Report') {
      parallel {
        stage('Report') {
          steps {
            emailext(subject: 'Terraform Regression - UserConnect-3.4.703 -100% Passed', body: 'tr-account, tr-gateway, tr-gateway_nat, tr-gateway_vpn, tr-gateway-vpn-nat, tr-gateway-ldap-duo', to: 'edsel@aviatrix.com')
          }
        }
        stage('slack') {
          steps {
            slackSend(message: 'Terraform Regression - $BUILD_DISPLAY_NAME -- 100% Passed', failOnError: true, token: 'zjC6JXcuigU1Nq0j3AoLBdci', teamDomain: 'aviatrix', baseUrl: 'https://aviatrix.slack.com/services/hooks/jenkins-ci/', channel: '#sitdown')
          }
        }
      }
    }
  }
}