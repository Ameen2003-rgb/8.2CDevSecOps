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
    bat 'echo "Skipping Snyk tests due to missing authentication"'
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