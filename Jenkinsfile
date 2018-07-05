node {
  def app

  stage('Clone repo') {
    checkout scm
  }

  stage('Build image') {
    docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
      app = docker.build("builder/apptest:${env.BUILD_ID}")
    }
  }

  stage('Push image') {
    docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
      docker.withRegistry('https://dtr.foolhq.com', 'dtr-builder') {
        app.push("${env.BUILD_ID}")
        app.push("latest")
        sh "env"
      }
    }
  }

//  stage('Deploy stack') {
//    docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
//      
//    }
    
}
