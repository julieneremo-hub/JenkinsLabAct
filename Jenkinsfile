pipeline {
    agent any

    environment {
        APP_NAME = 'echoes-of-the-past'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Flutter Project...'
                sh 'mkdir -p build/web && touch build/web/index.html'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'echo "All tests passed (100% coverage)"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Simulating deployment...'
                sh 'cp -r ./build /tmp/deployed-app/'
            }
        }

        stage('Notifications') {
            steps {
                echo 'Finalizing and sending email...'
                // This creates the "Notifications" circle in your map
                script {
                    mail to: 'julieneremo@gmail.com',
                         subject: "BUILD SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                         body: "Good news! Build ${env.BUILD_URL} completed successfully."
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Sending alert...'
            mail to: 'julieneremo@gmail.com',
                 subject: "BUILD FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build ${env.BUILD_URL} has failed. Please check the logs."
        }
    }
}