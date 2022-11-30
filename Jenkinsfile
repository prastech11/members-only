pipeline {
  agent any
  stages {
    stage('DockerBuild') {
      steps {
        echo 'DockerBuild'
        sh 'docker build -t testjenkins .'
        sh 'docker tag testjenkins:latest 315837431010.dkr.ecr.ap-south-1.amazonaws.com/testjenkins:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/testjenkins:latest'
      }
    }

  }
}