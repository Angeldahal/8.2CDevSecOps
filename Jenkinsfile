pipeline {
  agent any

  environment {
    TO_EMAIL = 'angeldahal202@gmail.com'
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Angeldahal/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        // Run tests but do not stop pipeline if they fail
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          bat 'npm test'
        }
      }
      post {
        success {
          emailext(
            to: "${TO_EMAIL}",
            subject: "Jenkins: ${JOB_NAME} #${BUILD_NUMBER} - TEST stage SUCCESS",
            body: "Stage: Run Tests\r\nStatus: SUCCESS\r\nJob: ${JOB_NAME}\r\nBuild: #${BUILD_NUMBER}\r\nURL: ${BUILD_URL}",
            attachLog: true,
            compressLog: true
          )
        }
        failure {
          emailext(
            to: "${TO_EMAIL}",
            subject: "Jenkins: ${JOB_NAME} #${BUILD_NUMBER} - TEST stage FAILURE",
            body: "Stage: Run Tests\r\nStatus: FAILURE\r\nJob: ${JOB_NAME}\r\nBuild: #${BUILD_NUMBER}\r\nURL: ${BUILD_URL}",
            attachLog: true,
            compressLog: true
          )
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        // Do not stop pipeline if coverage script fails
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          bat 'npm run coverage'
        }
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        // Do not stop pipeline if audit finds vulnerabilities
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          bat 'npm audit'
        }
      }
      post {
        success {
          emailext(
            to: "${TO_EMAIL}",
            subject: "Jenkins: ${JOB_NAME} #${BUILD_NUMBER} - SECURITY stage SUCCESS",
            body: "Stage: NPM Audit (Security Scan)\r\nStatus: SUCCESS\r\nJob: ${JOB_NAME}\r\nBuild: #${BUILD_NUMBER}\r\nURL: ${BUILD_URL}",
            attachLog: true,
            compressLog: true
          )
        }
        failure {
          emailext(
            to: "${TO_EMAIL}",
            subject: "Jenkins: ${JOB_NAME} #${BUILD_NUMBER} - SECURITY stage FAILURE",
            body: "Stage: NPM Audit (Security Scan)\r\nStatus: FAILURE\r\nJob: ${JOB_NAME}\r\nBuild: #${BUILD_NUMBER}\r\nURL: ${BUILD_URL}",
            attachLog: true,
            compressLog: true
          )
        }
      }
    }
  }
}
