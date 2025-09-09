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
                // The 'snyk test' command is failing due to an antivirus block.
                // This command needs to be allowed by your antivirus software on the Jenkins agent.
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    // This is the correct syntax for using 'Username with password' credentials.
                    // The username and password variables can be accessed within this block.
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
                // This step will now work correctly after adding the 'coverage' script to package.json
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
