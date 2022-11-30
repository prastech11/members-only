pipeline {
  agent any
  stages {
    stage('DockerBuild') {
      steps {
        echo 'DockerBuild'
        sh 'docker build -t new/new1 .'
      }
    }

  }
}