def app

pipeline {
  
  agent any

  environment {
    IMAGE = 'builder/apptest'
    LIVE_HOSTNAME = 'testapp-dev.docker.foolhq.com'
  }

  stages {
    stage('Clone repo') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
            app = docker.build("${IMAGE}:${env.BUILD_ID}")
          }
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
            docker.withRegistry('https://dtr.foolhq.com', 'dtr-builder') {
              app.push("${env.BUILD_ID}")
              app.push("latest")
            }
        }
        }
      }
    }

    stage('Deploy stack') {
      steps {
        script {
          docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
            sh "docker stack deploy -c docker-compose-dev.yml apptest-dev"
          }
        }
      }
    }
  }
}
