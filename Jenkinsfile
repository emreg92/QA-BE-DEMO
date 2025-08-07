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
        
        stage('Run Tests') {
            steps {
                echo 'Running test suite...'
                bat 'npm test'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
