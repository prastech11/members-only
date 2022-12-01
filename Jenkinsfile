pipeline {
  agent any
  stages {
    stage('Delete WorkSpace') {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, deleteDirs: true)
      }
    }

    stage('Build Docker') {
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q3x4k3p7'
        sh 'docker build -t jenkins .'
        sh 'docker tag jenkins:latest public.ecr.aws/q3x4k3p7/jenkins:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/jenkins:latest'
      }
    }

    stage('Put Files') {
      steps {
        readYaml(file: 'deployment.yaml', text: 'apiVersion: apps/v1 kind: Deployment metadata:   name: from-jenkins   namespace: default spec:   replicas: 2   selector:     matchLabels:       app: web   template:     metadata:       labels:         app: web     spec:       containers:         - name: back-end           image: public.ecr.aws/q3x4k3p7/jenkins           ports:             - containerPort: 3000')
        readYaml(file: 'service.yaml', text: 'apiVersion: v1 kind: Service metadata:   name: backend-service spec:   type: NodePort   selector:     app: web   ports:     - nodePort: 31480       port: 8080       targetPort: 3000')
      }
    }

    stage('K8s deploy') {
      steps {
        sh 'sudo aws eks --region ap-south-1 describe-cluster --name Dev-Test --query cluster.status'
        sh 'sudo aws eks --region ap-south-1 update-kubeconfig --name Dev-Test'
        sh 'sudo kubectl apply -f deployment.yaml'
        sh 'sudo kubectl apply -f service.yaml'
      }
    }

  }
}