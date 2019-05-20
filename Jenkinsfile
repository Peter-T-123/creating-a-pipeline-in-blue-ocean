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
        slackSend()
      }
    }
    stage('Post-Pipeline') {
      steps {
        slackSend(token: 'ahWUMnlmfuxO7UErPIrmwf2f', tokenCredentialId: 'sadface', color: 'green', channel: '#logs', message: 'Build Finished -$[env.JOB_NAME} ${ev.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)', baseUrl: 'devcops-workspace.slack.com')
      }
    }
  }
  environment {
    CI = 'true'
  }
}