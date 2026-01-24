pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Use the same branch that triggered the build
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devsecops-app:latest ./Application-Code'
            }
        }
        stage('SonarQube Scan') {
            steps {
                sh 'echo "SonarQube scan placeholder"'
            }
        }
        stage('Security Scan') {
            steps {
                sh 'echo "Trivy/Snyk scan placeholder"'
            }
        }
        stage('Deploy to EKS via ArgoCD') {
            steps {
                sh 'echo "Deploy placeholder"'
            }
        }
    }
}
