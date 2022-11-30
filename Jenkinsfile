pipeline {
  agent any
  stages {
    stage('Docker build') {
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --dev.pkhalatkar AWS --Welcome@12345-stdin public.ecr.aws/q3x4k3p7'
        sh 'docker build -t jenkins .'
        sh 'docker tag jenkins:latest public.ecr.aws/q3x4k3p7/jenkins:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/jenkins:latest'
      }
    }

  }
}