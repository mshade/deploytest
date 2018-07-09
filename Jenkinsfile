def app
pipeline {
  
  agent any

  environment {
    APP_NAME = "deploytest"
    APP_HOSTNAME = "${APP_NAME}-${BRANCH_NAME}.docker.foolhq.com"
    PROD_HOSTNAME = "${APP_NAME}.docker.foolhq.com"
    UCP_CREDS = "swarm-ucp-bundle"
    DTR_CREDS = "dtr-builder"
    IMAGE = "${DTR_URI}/builder/${APP_NAME}"
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
          docker.withServer("${UCP_URI}", "${UCP_CREDS}") {
            app = docker.build("${IMAGE}:${env.BUILD_ID}")
          }
        }
      }
    }

    stage('Push image') {
      steps {
        script {
          docker.withServer("${UCP_URI}", "${UCP_CREDS}") {
            docker.withRegistry("https://${DTR_URI}", "${DTR_CREDS}") {
              app.push("${env.BUILD_ID}")
              app.push("latest")
            }
          }
        }
      }
    }

    stage('Deploy to PreProd') {
      steps {
        script {
          docker.withServer("${UCP_URI}", "${UCP_CREDS}") {
            sh "docker stack deploy -c docker-compose-dev.yml ${APP_NAME}-${BRANCH_NAME}"
          }
        }
      }
    }

    stage('Deploy to Prod') {
      when { tag "release-*" }
      steps {
        script {
          docker.withServer("${UCP_URI}", "${UCP_CREDS}") {
            sh "docker stack deploy -c docker-compose-prod.yml ${APP_NAME}-release"
          }
        }
      }
    }
  }
}
