pipeline {
  agent any
  
  stages {
    stage('Verificar tools') {
      steps {
        sh 'docker info'
      }
    }

    stage('Build docker image') {
      steps {
        sh 'docker build -t app-test-jenkins .'
      }
    }

    stage('Run docker image') {
      steps {
        sh 'docker run -dit --name app-test-jenkins app-test-jenkins'
      }
    }

    stage('Run specs') {
      steps {
        sh 'docker exec app-test-jenkins ./vendor/bin/phpunit tests'
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