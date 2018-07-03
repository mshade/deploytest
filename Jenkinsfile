pipeline {
    options {
      dockerNode(credentialsId: 'swarm-ucp-bundle', dockerHost: 'tcp://docker.foolhq.com:443', image: '', remoteFs: '')
    }
    agent {
      docker {
        image 'node:7-alpine'
      }
    }
      
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
