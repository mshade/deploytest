pipeline {
    agent {
      docker.withserver('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') { 
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
