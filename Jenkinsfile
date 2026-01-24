pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kofifreeman1s3-spec/DevOps.git'
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
