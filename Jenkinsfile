pipeline {
  agent any
  environment {
    PATH = "/usr/local/go/bin:/home/ubuntu/automation_test_scripts/Terraform_Scripts/gateway_vpn_ldap_duo:$PATH"
  }
  stages {
    stage('Build') {
      steps {
        sh 'echo hello'
      }
    }
    stage('Test') {
      steps {
        sh 'echo "test stage path is: $PATH"'
        sh '''
           pwd
           ls -ltr
           cd /home/ubuntu/automation_test_scripts/Terraform_Scripts/gateway_vpn_ldap_duo
           pwd
           ls -ltr
        '''
      }
    }
    stage('Report') {
      steps {
        mail(subject: 'pipeline1 test', body: 'hello', from: 'localhost', to: 'edsel@aviatrix.com')
      }
    }
  }
}
