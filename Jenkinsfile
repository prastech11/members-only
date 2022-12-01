pipeline {
  agent any
  stages {
    stage('Docker Build') {
      steps {
        sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/q3x4k3p7'
        sh 'docker build -t jenkins .'
        sh 'docker tag jenkins:latest public.ecr.aws/q3x4k3p7/jenkins:latest'
        sh 'docker push public.ecr.aws/q3x4k3p7/jenkins:latest'
      }
    }

    stage('Put Dep and Srv File') {
      steps {
        prependToFile(file: 'deployment.yaml', content: 'apiVersion: apps/v1 kind: Deployment metadata:   name: test1-server   namespace: default spec:   replicas: 2   selector:     matchLabels:       app: web   template:     metadata:       labels:         app: web     spec:       containers:         - name: back-end           image: public.ecr.aws/q3x4k3p7/wecan1           ports:             - containerPort: 3000')
        prependToFile(file: 'service.yaml', content: 'apiVersion: v1 kind: Service metadata:   name: backend-service spec:   type: NodePort   selector:     app: web   ports:     - nodePort: 31480       port: 8080       targetPort: 3000')
      }
    }

    stage('Deploy to K8s') {
      steps {
        sh 'aws eks --region ap-south-1 describe-cluster --name Dev-Test --query cluster.status'
        sh '''
aws eks --region ap-south-1 update-kubeconfig --name Dev-Test'''
        sh 'sudo kubectl apply -f deployment.yaml'
      }
    }

  }
}