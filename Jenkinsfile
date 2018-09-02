pipeline {
  agent any
  stages {
    stage('Stage1') {
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
    stage('Stage2') {
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
    stage('Stage3') {
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
            build(job: 'tr-gateway-ldap-duo', propagate: true, wait: true, quietPeriod: 60)
          }
        }
      }
    }
    stage('Stage4') {
      parallel {
        stage('Site2Cloud Test') {
          steps {
            addBadge(icon: 'Site2Cloud', text: 'Site2Cloud Test')
          }
        }
        stage('Site2Cloud') {
          steps {
            build(job: 'tr-s2c-vgw', propagate: true, wait: true, quietPeriod: 60)
          }
        }
        stage('Site2CloudHA') {
          steps {
            build(job: 'tr-s2cHA-vgw', propagate: true, quietPeriod: 10, wait: true)
          }
        }
      }
    }
    stage('Stage5') {
      steps {
        addBadge(icon: 'FQDN', text: 'FQDN Test')
      }
    }
    stage('Stage6') {
      steps {
        addBadge(icon: 'Access Account Test', text: 'Access Account')
      }
    }
    stage('Report') {
      parallel {
        stage('Report') {
          steps {
            echo 'Report Results'
            emailext(subject: '$BUILD_TAG', body: '$BUILD_DISPLAY', to: 'edsel@aviatrix.com', attachLog: true)
          }
        }
        stage('slack') {
          steps {
            slackSend(message: 'Terraform Regression - 100% Passed', baseUrl: ' https://aviatrix.slack.com/services/hooks/jenkins-ci/', token: 'zjC6JXcuigU1Nq0j3AoLBdci', teamDomain: 'aviatrix', channel: '#sitdown')
          }
        }
      }
    }
  }
}