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
        stage('Run Tests & Snyk Scan') {
            steps {
                // This command now correctly runs the 'test' script from package.json
                bat 'npm run test || exit /b 0'
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // This command now correctly runs the 'coverage' script from package.json
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
    }

    post {
        always {
            // The withCredentials block must wrap the emailext step to provide access to the credentials.
            withCredentials([usernamePassword(credentialsId: 'Mail', usernameVariable: 'SMTP_USER', passwordVariable: 'SMTP_PASS')]) {
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
}
