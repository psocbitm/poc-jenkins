pipeline {
    agent any
    tools {
        nodejs 'node' // Ensure you have Node.js configured in Jenkins global tools
    }
    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/psocbitm/poc-jenkins.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh 'npm install'
            }
        }
        stage('Lint') {
            steps {
                // Run ESLint
                sh 'npm run lint'
            }

        }
        stage('Build') {
            steps {
                // Build the React project
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
            post {
                always {
                    // Archive test results
                    junit '**/test-results.xml'
                }
            }
        }

    }
    post {
        always {
            // Clean up workspace
            cleanWs()

            // Archive build artifacts
            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true

            // Notify on build status
            emailext to: 'palashasati1@gmail.com',
                    subject: "Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "Build ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}\n\n" +
                          "Check console output at ${env.BUILD_URL} to view the results."
        }
        failure {
            // Send failure notifications
            emailext to: 'palashasati1@gmail.com',
                    subject: "FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    body: "Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}\n\n" +
                          "Check console output at ${env.BUILD_URL} to view the results."
        }
    }
}
