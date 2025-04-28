pipeline {
    agent {
        docker { 
            image 'node:23-alpine3.20' 
            args '-u root:root' // Run as root user to avoid permission issues}
        }
    }

    stages {
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