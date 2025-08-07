pipeline {
    agent any
    
    environment {
        PORT = '8080'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from repository...'
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                bat 'npm ci'
            }
        }
        
        stage('Start Application') {
            steps {
                echo 'Starting the application...'
                bat 'start /B npm start'
                bat 'ping -n 15 127.0.0.1 > nul'
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Running test suite...'
                bat 'npm test'
            }
        }
        
        stage('Cleanup') {
            steps {
                echo 'Cleaning up processes...'
                bat 'taskkill /f /im node.exe 2>nul || exit 0'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed'
            bat 'taskkill /f /im node.exe 2>nul || exit 0'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
