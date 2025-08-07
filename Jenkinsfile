pipeline {
    agent any
    
    environment {
        NODE_VERSION = '18'
        PORT = '8080'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from repository...'
                checkout scm
            }
        }
        
        stage('Setup Node.js') {
            steps {
                echo 'Setting up Node.js environment...'
                nodejs(nodeJSInstallationName: 'NodeJS-18') {
                    sh 'node --version'
                    sh 'npm --version'
                }
            }
        }
        
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                nodejs(nodeJSInstallationName: 'NodeJS-18') {
                    sh 'npm ci'
                }
            }
        }
        
        stage('Start Application') {
            steps {
                echo 'Starting the application...'
                nodejs(nodeJSInstallationName: 'NodeJS-18') {
                    sh 'npm start &'
                    sh 'sleep 10'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                echo 'Running test suite...'
                nodejs(nodeJSInstallationName: 'NodeJS-18') {
                    sh 'npm test'
                }
            }
        }
        
        stage('Test Application Health') {
            steps {
                echo 'Testing application health...'
                script {
                    try {
                        sh 'curl -f http://localhost:8080 || exit 1'
                        echo 'Application is responding correctly'
                    } catch (Exception e) {
                        echo 'Application health check failed'
                        error 'Application is not responding'
                    }
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                echo 'Cleaning up processes...'
                sh 'pkill -f "node index.js" || true'
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
        cleanup {
            sh 'pkill -f "node index.js" || true'
        }
    }
}
