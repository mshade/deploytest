node {
  def app

  stage('Clone repo') {
    checkout scm
  }

  stage('Build image') {
    docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
      app = docker.build("builder/apptest:${env.BUILD_ID}")
      app.inside {
        sh 'echo success'
      }
    }
  }
}
