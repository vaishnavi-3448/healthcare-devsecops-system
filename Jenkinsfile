pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/vaishnavi-3448/healthcare-devsecops.git'
            }
        }

        stage('Code Security Scan') {
            steps {
                bat 'pip install bandit'
                bat 'bandit -r app/'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t healthcare-app:latest .'
            }
        }

        stage('Container Security Scan') {
            steps {
                bat 'docker run --rm aquasec/trivy:latest image python:3.10-slim --scanners vuln'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yaml'
                bat 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}