pipeline {
  agent any
  stages {
    stage('Stage1') {
      parallel {
        stage('Build, Upgrade, Account') {
          steps {
            addHtmlBadge 'Terraform Regression 3.4'
            build(propagate: true, job: 'tr-build_aviatrix', quietPeriod: 10, wait: true)
            build(job: 'pylint-check', propagate: true, quietPeriod: 10, wait: true)
          }
        }
        stage('tr-upgrade') {
          steps {
            build(job: 'upgrade_only', propagate: true, quietPeriod: 10)
          }
        }
        stage('account') {
          steps {
            build(job: 'tr-account', propagate: true, quietPeriod: 10)
            build(job: 'tr-iam-account', propagate: true, quietPeriod: 10, wait: true)
            build(job: 'tr-role-account', propagate: true, quietPeriod: 10, wait: true)
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
        stage('Gateway NAT') {
          steps {
            build(job: 'tr-gateway', propagate: true, quietPeriod: 10, wait: true)
            sleep 10
            build(job: 'tr-gateway-nat', propagate: true, quietPeriod: 10, wait: true)
          }
        }
        stage('Gateway AZ HA') {
          steps {
            build(job: 'tr-gateway-single-AZ-ha', propagate: true, quietPeriod: 20, wait: true)
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
            sleep 30
            build(job: 'tr-s2cHA-vgw', propagate: true, quietPeriod: 20, wait: true)
          }
        }
      }
    }
    stage('Stage5') {
      parallel {
        stage('Stage5') {
          steps {
            addBadge(icon: 'FQDN', text: 'FQDN Test')
          }
        }
        stage('aws-peering') {
          steps {
            build(job: 'tr-aws-peering', propagate: true, quietPeriod: 20, wait: true)
            sleep 10
            build(job: 'tr-aws-peering', propagate: true, quietPeriod: 10, wait: true)
            sleep 10
            build(job: 'tr-fqdn3.3', propagate: true, quietPeriod: 10, wait: true)
            sleep 10
          }
        }
      }
    }
    stage('Stage6') {
      parallel {
        stage('Stage6') {
          steps {
            addBadge(icon: 'Access Account Test', text: 'Access Account')
          }
        }
        stage('peering') {
          steps {
            build(job: 'tr-peeringHA', propagate: true, quietPeriod: 10, wait: true)
          }
        }
      }
    }
    stage('Stage7') {
      parallel {
        stage('Firewall') {
          steps {
            echo 'Report Results'
            emailext(subject: '$BUILD_TAG - $BUILD_NAME', body: 'Terraform Regression - 100% Passed', to: 'edsel@aviatrix.com', attachLog: true)
          }
        }
        stage('firewall-policy') {
          steps {
            build(job: 'tr-firewall-tag', propagate: true, quietPeriod: 5, wait: true)
          }
        }
        stage('firewall-tag') {
          steps {
            build(job: 'tr-firewall-tag', propagate: true, quietPeriod: 5, wait: true)
          }
        }
        stage('firewall-policy-tag') {
          steps {
            build(job: 'tr-firewall-policy-tag', propagate: true, quietPeriod: 5, wait: true)
          }
        }
      }
    }
    stage('Stage8') {
      parallel {
        stage('FQDN') {
          steps {
            addInfoBadge 'Stage8 - FQDN Test'
          }
        }
        stage('tr-fqdn3.4') {
          steps {
            build(job: 'tr-fqdn3.4', propagate: true, quietPeriod: 10, wait: true)
          }
        }
      }
    }
    stage('Slack') {
      steps {
        slackSend(token: 'zjC6JXcuigU1Nq0j3AoLBdci', failOnError: true, teamDomain: 'aviatrix', channel: '#test-terraform-reg', baseUrl: 'https://aviatrix.slack.com/services/hooks/jenkins-ci/', message: 'Terraform Regression - 100% Passed', attachments: 'upgrade, account, gateway, nat, az ha, site2cloud, aws peering, peeringHA, firewall')
        emailext(subject: 'Terraform Regression - 100% Passed', body: 'Aviatrix terraform provider module regression', from: 'noreply@aviatrix.com', replyTo: 'edsel@aviatrix.com')
      }
    }
  }
}