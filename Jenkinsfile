pipeline {
    agent none

    environment {
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub-credentials'
        DOCKER_IMAGE = 'yourdockerhubusername/your-app'
    }

    stages {
        agent {
            docker {
                image 'sonarsource/sonar-scanner-cli:11.3'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                checkout scm
            }
            steps {
                sh '''
                sonar-scanner \
                  -Dsonar.projectKey=my-node-app \
                  -Dsonar.sources=. \
                '''
            }
        }
    }
    stages {
        agent {
            docker { 
                image 'node:23-alpine3.20' 
                args '-u root:root'
            }
        }
        stage('Clone Repository') {
            when {
                changeset "**/src/**"
            }
            steps {
                checkout scm
            }
        }    
        stage('Install Dependencies') {
            when {
                changeset "**/src/**"
            }
            steps {
                sh 'npm ci'
            }
        }
        stage('Build') {
            when {
                changeset "**/src/**"
            }
            steps {
                sh 'npm run build --if-present'
            }
        }
        stage('Run Tests') {
            when {
                changeset "**/src/**"
            }
            steps {
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