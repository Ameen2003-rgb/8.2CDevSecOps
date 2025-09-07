pipeline {
  agent any
  stages {
    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }
    stage('Run Tests') {
  steps {
    bat 'npm test'
  }
}
stage('Generate Coverage Report') {
  steps {
    bat 'npm run coverage'
  }
}
stage('NPM Audit (Security Scan)') {
  steps {
    bat 'npm audit'
  }
}
  }
}