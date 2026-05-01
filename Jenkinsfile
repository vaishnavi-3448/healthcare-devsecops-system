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
        bat '''
        docker run --rm ^
        -v //var/run/docker.sock:/var/run/docker.sock ^
        aquasec/trivy:latest image healthcare-app:latest
        '''
    }
}

        stage('Deploy to Kubernetes') {
    steps {
        bat 'kubectl apply -f k8s/deployment.yaml --validate=false'
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