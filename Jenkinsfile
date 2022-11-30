pipeline {
  agent any
  stages {
    stage('DockerBuild') {
      steps {
        echo 'DockerBuild'
        sh 'docker build -t new/new1 .'
        sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 315837431010.dkr.ecr.ap-south-1.amazonaws.com'
        sh 'docker build -t testjenkins .'
        sh 'docker tag testjenkins:latest 315837431010.dkr.ecr.ap-south-1.amazonaws.com/testjenkins:latest'
        sh 'docker push 315837431010.dkr.ecr.ap-south-1.amazonaws.com/testjenkins:latest'
      }
    }

  }
}