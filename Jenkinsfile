pipeline {
    agent none

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub-credentials'
        DOCKER_IMAGE = 'yourdockerhubusername/your-app'
    }

    stages {
        stage('SonarQube Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:11.3'
                }
            }
            steps {
                checkout scm
                
                sh '''
                sonar-scanner \
                  -Dsonar.projectKey=front-end \
                  -Dsonar.sources=. \
                '''
            }
        }

        stage('Unit Tests') {
            agent {
                docker { 
                    image 'node:23-alpine3.20' 
                    args '-u root:root'
                }
            }
            when {
                changeset "**/src/**"
            }
            steps {
                checkout scm
                sh 'npm ci'
                sh 'npm test -- --watchAll=false'
            }
        }    
    }

    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}