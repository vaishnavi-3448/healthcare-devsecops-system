pipeline {
    agent any

    stages {

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
        bat 'trivy image --scanners vuln healthcare-app:latest'
    }
}

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/deployment.yaml'
                bat 'kubectl apply -f k8s/service.yaml'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'kubectl get pods'
                bat 'kubectl get svc'
            }
        }
    }
}