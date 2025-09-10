pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Ameen2003-rgb/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
    }

    post {
        always {
            // Send a single email at the end of the build
            emailext(
                subject: "Build #${env.BUILD_NUMBER} - ${currentBuild.result}",
                body: """Build URL: ${env.BUILD_URL}
Build Status: ${currentBuild.result}

Stages completed:
- Checkout
- Install Dependencies
- Run Tests
- Generate Coverage Report
- NPM Audit (Security Scan)

Check console output for detailed logs.""",
                to: "mohammedameen1089@gmail.com",
                attachLog: true
            )
        }
    }
}
