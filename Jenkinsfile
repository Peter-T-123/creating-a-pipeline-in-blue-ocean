pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '''-p 3000:3000
--volume=C:\\/Program Files (x86)\\/Jenkins\\/workspace\\/proofofconceptpipeline_master'''
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
  }
  environment {
    CI = 'true'
    MSYS_NO_PATHCONV = '1'
  }
}