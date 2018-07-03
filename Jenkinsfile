pipeline {
    agent {
      docker {
        // This step should not normally be used in your script. Consult the inline help for details.
        withDockerServer([credentialsId: 'swarm-ucp-bundle', uri: 'tcp://docker.foolhq.com:443']) {
            // some block
        }

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
