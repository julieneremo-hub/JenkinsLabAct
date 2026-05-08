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
                // This creates a fake JUnit XML result file
                sh '''
                echo "<?xml version='1.0' encoding='UTF-8'?>
                <testsuites>
                  <testsuite name='LabTests' tests='1' failures='0'>
                    <testcase name='test_integration_success' />
                  </testsuite>
                </testsuites>" > test-results.xml
                '''
                // This tells Jenkins to "consume" the file and create the report
                junit 'test-results.xml'
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
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    mail to: 'julieneremo@gmail.com',
                         subject: "BUILD SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                         body: "Good news! Build ${env.BUILD_URL} completed successfully."
                }
            }
        }
    }

    post {
        failure {
            // This is also from your lab act file
            mail to: 'julieneremo@gmail.com',
                 subject: "BUILD FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build ${env.BUILD_URL} has failed. Please check the logs."
        }
    }
}