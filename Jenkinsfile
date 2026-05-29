pipeline {
    agent any

    environment {
        IMAGE_NAME = "192.168.56.30:5000/flask-app"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Clone Source Code') {
            steps {
                git 'https://github.com/YOUR_USERNAME/hello-world.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME:$TAG .
                '''
            }
        }

        stage('Push Image to Registry') {
            steps {
                sh '''
                docker push $IMAGE_NAME:$TAG
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                sed -i "s|IMAGE_PLACEHOLDER|$IMAGE_NAME:$TAG|g" k8s/deployment.yaml

                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
