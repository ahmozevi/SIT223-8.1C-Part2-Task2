// Retry at 8:17 PM AEST
pipeline {
  agent any
  triggers { pollSCM('* * * * *') }  // poll every minute
  // Optional: set a default recipient here, or rely on "Default Recipients" in Extended E-mail Notification
  environment { TO_EMAIL = 'ahmozevi@gmail.com' }  // <-- change or remove if you set Default Recipients

  stages {
    stage('Build') {
      steps { echo 'Task: Compile & package | Tool: Maven (or Gradle/npm)' }
    }

    stage('Unit and Integration Tests') {
      steps {
        echo 'Task: Run unit + integration tests | Tools: JUnit/Jest/Mocha'
      }
      post {
        success {
          emailext(
            // omit "to:" if you set Default Recipients in Jenkins global config
            to: env.TO_EMAIL,
            subject: "Unit & Integration Tests SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """Stage: Unit & Integration Tests
Status: SUCCESS
Build URL: ${env.BUILD_URL}
(See attached console log)
""",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
        failure {
          emailext(
            to: env.TO_EMAIL,
            subject: "Unit & Integration Tests FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """Stage: Unit & Integration Tests
Status: FAILURE
Build URL: ${env.BUILD_URL}
(See attached console log)
""",
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
            body: """Stage: Security Scan
Status: SUCCESS
Build URL: ${env.BUILD_URL}
(See attached console log)
""",
            attachLog: true, compressLog: true, mimeType: 'text/plain'
          )
        }
        failure {
          emailext(
            to: env.TO_EMAIL,
            subject: "Security Scan FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """Stage: Security Scan
Status: FAILURE
Build URL: ${env.BUILD_URL}
(See attached console log)
""",
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
