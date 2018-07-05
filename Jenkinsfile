def app

pipeline {
  
  agent any

  environment {
    IMAGE = "${DTR_URI}/builder/apptest"
    LIVE_HOSTNAME = testapp-${GIT_BRANCH}.docker.foolhq.com"
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
          docker.withServer("${UCP_URI}", 'swarm-ucp-bundle') {
            app = docker.build("${IMAGE}:${env.BUILD_ID}")
          }
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withServer("${UCP_URI}", 'swarm-ucp-bundle') {
            docker.withRegistry("https://${DTR_URI}", 'dtr-builder') {
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
          docker.withServer("${UCP_URI}", 'swarm-ucp-bundle') {
            sh "docker stack deploy -c docker-compose-dev.yml apptest-dev"
          }
        }
      }
    }
  }
}
