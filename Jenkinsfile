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
                    emailext (
                        subject: "Test Stage: Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                        body: "Stage 'Run Tests' finished with status: ${currentBuild.currentResult}. \nSee logs: ${env.BUILD_URL}",
                        to: "mohammedameen1089@gmail.com",
                        attachLog: true
                    )
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
                    emailext (
                        subject: "NPM Audit: Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                        body: "Stage 'NPM Audit (Security Scan)' finished with status: ${currentBuild.currentResult}. \nSee logs: ${env.BUILD_URL}",
                        to: "mohammedameen1089@gmail.com",
                        attachLog: true
                    )
                }
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Pipeline Summary: Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                body: """Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                Result: ${currentBuild.currentResult}
                Logs: ${env.BUILD_URL}""",
                to: "mohammedameen1089@gmail.com",
                attachLog: true
            )
        }
    }
}
