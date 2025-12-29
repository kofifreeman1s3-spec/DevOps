pipeline {
    agent any

    tools {
        maven "maven3"
    }

    environment {
        registry = "18.234.241.164:8082/vprofile"
        registryCredential = "nexus-registry"
    }

    stages {

        stage('FETCH CODE') {
            steps {
                checkout scm
            }
        }

        stage('BUILD') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('UNIT TEST') {
            steps {
                sh 'mvn test'
            }
        }

        stage('PACKAGE') {
            steps {
                sh 'mvn package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('CODE ANALYSIS WITH CHECKSTYLE') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('CODE ANALYSIS WITH SONARQUBE') {
            environment {
                scannerHome = tool 'mysonarscanner4'
            }
            steps {
                withSonarQubeEnv('sonar') {
                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile \
                    -Dsonar.projectVersion=1.0
                    """
                }
            }
        }

        stage('BUILD DOCKER IMAGE') {
            steps {
                sh 'docker build -t vprofile:latest .'
            }
        }

        stage('PUSH IMAGE TO NEXUS') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: registryCredential,
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh """
                    docker login ${registry} -u ${NEXUS_USER} -p ${NEXUS_PASS}
                    docker tag vprofile:latest ${registry}:latest
                    docker push ${registry}:latest
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ PIPELINE SUCCESSFUL — BUILD IS GREEN!'
        }
        failure {
            echo '❌ PIPELINE FAILED — CHECK LOGS'
        }
    }
}

