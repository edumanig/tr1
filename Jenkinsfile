pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        sh 'cd /home/ubuntu/automation_test_scripts/Terraform_Scripts/gateway_vpn_ldap_duo'
        sh 'terraform plan -var-file=/home/ubuntu/54.177.55.54.secret_tr_regression.tfvars '
      }
    }
    stage('Report') {
      steps {
        mail(subject: 'pipeline1 test', body: 'hello', from: 'localhost', to: 'edsel@aviatrix.com')
      }
    }
  }
}
