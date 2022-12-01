pipeline {
  agent any
  stages {
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
        prependToFile(file: 'deploymnet.yaml', content: 'apiVersion: apps/v1 kind: Deployment metadata:   name: from-jenkins   namespace: default spec:   replicas: 2   selector:     matchLabels:       app: web   template:     metadata:       labels:         app: web     spec:       containers:         - name: back-end           image: public.ecr.aws/q3x4k3p7/jenkins           ports:             - containerPort: 3000')
        prependToFile(file: 'service.yaml', content: 'apiVersion: v1 kind: Service metadata:   name: backend-service spec:   type: NodePort   selector:     app: web   ports:     - nodePort: 31479       port: 8080       targetPort: 3000')
      }
    }

    stage('K8s deploy') {
      steps {
        sh 'sudo aws eks --region ap-south-1 describe-cluster --name Dev-Test --query cluster.status'
        sh 'sudo aws eks --region ap-south-1 update-kubeconfig --name Dev-Test'
        writeYaml(data: 'apiVersion: apps/v1 kind: Deployment metadata:   name: from-jenkins   namespace: default spec:   replicas: 2   selector:     matchLabels:       app: web   template:     metadata:       labels:         app: web     spec:       containers:         - name: back-end           image: public.ecr.aws/q3x4k3p7/jenkins           ports:             - containerPort: 3000', file: 'deployment.yaml', overwrite: true)
        sh 'sudo kubectl apply -f deployment.yaml --validate=false'
        sh 'sudo kubectl apply -f service.yaml'
      }
    }

  }
}