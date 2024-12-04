pipeline {
  // agent { label 'ubuntu' }
  agent any

  environment {
    SONAR_TOKEN = credentials('sonarqube')
  }
  
  stages {
    stage('Verificar tools') {
      steps {
        sh 'docker info'
      }
    }

    // stage('Build docker image') {
    //   steps {
    //     sh 'docker build -t app-test-jenkins .'
    //   }
    // }

    stage('Run docker') {
      steps {
        // sh 'docker run -dit --name app-test-jenkins app-test-jenkins'
        sh 'docker run -dit --name app-jenkins app-jenkins'
      }
    }

    stage('Run specs') {
      steps {
        // sh 'docker exec app-test-jenkins ./vendor/bin/phpunit tests'
        sh 'docker exec app-jenkins ./vendor/bin/phpunit tests'
      }
    }

    stage ('sonarqube') {
      steps {
        script {
          docker.image('sonarsource/sonar-scanner-cli:5.0.1').inside('--network ci-network') {
            sh '''
              sonar-scanner \
                -Dsonar.host.url=http://sonarqube:9000 \
                -Dsonar.projectKey=jenkins-php \
                -Dsonar.src=src \
                -Dsonar.token=$SONAR_TOKEN
            '''
          }
        }
      }
    }

    // stage('Deploy') {
    //   steps {
    //     sh './deploy.sh'
    //   }
    // }
  }

  post {
    always {
      // sh 'docker stop app-test-jenkins'
      // sh 'docker rm app-test-jenkins'
      sh 'docker stop app-jenkins'
      sh 'docker rm app-jenkins'
    }

    // success {
    //   slackSend(channel: '#tutorial', message: 'SUCCESS! test')
    // }

    // failure {
    //   slackSend(channel: '#tutorial', message: 'FAIL! test')
    // }
  }
}