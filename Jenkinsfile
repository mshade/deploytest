node {
  checkout scm

  docker.withServer('tcp://docker.foolhq.com:443', 'swarm-ucp-bundle') {
    docker.image('node:7-alpine') {
      sh node --version
      }
  }
}
