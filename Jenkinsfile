pipeline {
  agent { label 'ubuntu' }
  
  stages {
    stage('Verificar tools') {
      steps {
        sh 'docker info'
      }
    }
  }

  post {
    always {
      sh 'docker stop app-test-jenkins'
      sh 'docker rm app-test-jenkins'
    }

    success {
      slackSend(channel: '#tutorial', message: 'SUCCESS! test')
    }
  }
}