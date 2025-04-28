pipeline {
    agent {
        docker { 
            image 'node:23-alpine3.20' 
            args '-u root:root' // Run as root user to avoid permission issues}
        }
    }
    
    triggers {
        pollSCM('* * * * *') // Check for changes every minute
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }    
        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build --if-present'
            }
        }
        stage('Run Tests') {
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