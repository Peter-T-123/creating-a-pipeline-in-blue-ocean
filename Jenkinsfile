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
        slackSend(token: 'ahWUMnlmfuxO7UErPIrmwf2f', tokenCredentialId: 'sadface', channel: '#logs', message: "Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}", baseUrl: 'https://devcops-workspace.slack.com/services/hooks/jenkins-ci/', teamDomain: 'https://devcops-workspace.slack.com', attachments: 'text: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\\n More info at: ${env.BUILD_URL}')
      }
    }
  }
  environment {
    CI = 'true'
  }
}