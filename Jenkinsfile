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
            post {
                always {
                    withCredentials([usernamePassword(credentialsId: 'Mail', usernameVariable: 'SMTP_USER', passwordVariable: 'SMTP_PASS')]) {
                        emailext (
                            subject: "Test Stage: Build #${env.BUILD_NUMBER} - ${currentBuild.result}",
                            body: "Stage 'Run Tests' finished with status: ${currentBuild.result}.\nCheck the full console log at: ${env.BUILD_URL}",
                            to: "mohammedameen1089@gmail.com",
                            attachLog: true
                        )
                    }
                }
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
            post {
                always {
                    withCredentials([usernamePassword(credentialsId: 'Mail', usernameVariable: 'SMTP_USER', passwordVariable: 'SMTP_PASS')]) {
                        emailext (
                            subject: "NPM Audit: Build #${env.BUILD_NUMBER} - ${currentBuild.result}",
                            body: "Stage 'NPM Audit (Security Scan)' finished with status: ${currentBuild.result}.\nCheck the full console log at: ${env.BUILD_URL}",
                            to: "mohammedameen1089@gmail.com",
                            attachLog: true
                        )
                    }
                }
            }
        }
    }
}
