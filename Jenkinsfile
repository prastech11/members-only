pipeline {
  agent any
  stages {
    stage('WorkSpace Clean') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true)
      }
    }

    stage('Docker Build') {
      steps {
        sh 'docker build -t jenkins .'
        sh 'docker tag jenkins:latest public.ecr.aws/q3x4k3p7/jenkins:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/jenkins:latest'
      }
    }

  }
}