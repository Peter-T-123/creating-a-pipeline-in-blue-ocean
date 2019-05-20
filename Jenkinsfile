pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3000:3000'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh './jenkins/scripts/test.sh'
      }
    }
    stage('Deliver') {
      steps {
        sh './jenkins/scripts/deliver.sh'
        input 'Finished using the web site? (Click "Proceed" to continue)'
        sh './jenkins/scripts/kill.sh'
      }
    }
    stage('Post-Pipeline') {
      steps {
        slackSend(token: 'ahWUMnlmfuxO7UErPIrmwf2f', tokenCredentialId: 'sadface', channel: '#logs', message: 'Build Started -  Project: $JOB_NAME Build Nummer: BUILD_NUMBER ($BUILD_URL)', baseUrl: 'https://devcops-workspace.slack.com/services/hooks/jenkins-ci/', teamDomain: 'https://devcops-workspace.slack.com')
      }
    }
  }
  environment {
    CI = 'true'
  }
}