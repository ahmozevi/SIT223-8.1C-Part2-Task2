pipeline {
  agent any
  triggers { pollSCM('* * * * *') }
  environment { TO_EMAIL = 'ahmozevi@gmail.com' } // or set Default Recipients globally

  stages {
    stage('Build') {
      steps { echo 'Task: Compile & package | Tool: Maven (or Gradle/npm)' }
    }

    stage('Unit and Integration Tests') {
      steps { echo 'Task: Run unit + integration tests | Tools: JUnit/Jest/Mocha' }
      post {
        success {
          emailext(
            to: env.TO_EMAIL,
            subject: "Unit & Integration Tests SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage: Unit & Integration Tests\nStatus: SUCCESS\nBuild URL: ${env.BUILD_URL}\n",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
        failure {
          emailext(
            to: env.TO_EMAIL,
            subject: "Unit & Integration Tests FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage: Unit & Integration Tests\nStatus: FAILURE\nBuild URL: ${env.BUILD_URL}\n",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
      }
    }

    stage('Code Analysis') {
      steps { echo 'Task: Static code analysis | Tools: SonarQube/SonarCloud/ESLint' }
    }

    stage('Security Scan') {
      steps { echo 'Task: Vulnerability scan | Tools: OWASP Dependency-Check/Snyk/npm audit' }
      post {
        success {
          emailext(
            to: env.TO_EMAIL,
            subject: "Security Scan SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage: Security Scan\nStatus: SUCCESS\nBuild URL: ${env.BUILD_URL}\n",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
        failure {
          emailext(
            to: env.TO_EMAIL,
            subject: "Security Scan FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: "Stage: Security Scan\nStatus: FAILURE\nBuild URL: ${env.BUILD_URL}\n",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
      }
    }

    stage('Deploy to Staging') {
      steps { echo 'Task: Deploy to staging (e.g., AWS EC2) | Tools: Ansible/Docker/K8s' }
    }
    stage('Integration Tests on Staging') {
      steps { echo 'Task: Run integration tests on staging | Tools: Postman/Newman/Cypress/Selenium' }
    }
    stage('Deploy to Production') {
      steps { echo 'Task: Deploy to production (e.g., AWS EC2) | Tools: Ansible/Docker/K8s' }
    }
  }
}
