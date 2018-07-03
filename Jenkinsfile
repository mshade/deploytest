pipeline {
    agent {
      docker { 
        image 'node:7-alpine'
        withServer 'tcp://docker.foolhq.com:443' 'swarm-ucp-bundle'
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
