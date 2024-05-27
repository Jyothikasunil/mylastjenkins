pipeline {
    agent any

    environment {
        NOTIFICATION_EMAIL = "jyothikasunil006@gmail.com"
        LOG_FILE = "${env.WORKSPACE}/pipeline.log"
    }

    stages {
        stage("Build") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Building the code using Maven" | tee ${logFilePath}
                        # Add actual build commands here, for example:
                        # mvn clean install | tee -a ${logFilePath}
                    """
                }
            }
        }

        stage("Unit and Integration Tests") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Running unit tests with JUnit" | tee -a ${logFilePath}
                        # Add unit test commands here, for example:
                        # mvn test | tee -a ${logFilePath}
                        echo "Running integration tests with Selenium" | tee -a ${logFilePath}
                        # Add integration test commands here, for example:
                        # mvn verify -Pintegration-tests | tee -a ${logFilePath}
                    """
                }
            }
            post {
                success {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Unit and Integration tests completed successfully. See attached log for details.',
                             subject: 'Unit and Integration Tests: SUCCESS',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
                failure {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Unit and Integration tests failed. See attached log for details.',
                             subject: 'Unit and Integration Tests: FAILURE',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
            }
        }

        stage("Code Analysis") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Running code analysis with SonarQube" | tee -a ${logFilePath}
                        # Add code analysis commands here, for example:
                        # sonar-scanner | tee -a ${logFilePath}
                    """
                }
            }
        }

        stage("Security Scan") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Running security scan with OWASP ZAP" | tee -a ${logFilePath}
                        # Add security scan commands here, for example:
                        # zap-cli quick-scan --self-contained | tee -a ${logFilePath}
                    """
                }
            }
            post {
                success {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Security Scan completed successfully. See attached log for details.',
                             subject: 'Security Scan: SUCCESS',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
                failure {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Security Scan failed. See attached log for details.',
                             subject: 'Security Scan: FAILURE',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
            }
        }

        stage("Deploy to Staging") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Deploying the application to staging server using AWS CodeDeploy" | tee -a ${logFilePath}
                        # Add staging deployment commands here, for example:
                        # aws deploy push --application-name my-app --s3-location s3://my-bucket/my-app.zip | tee -a ${logFilePath}
                    """
                }
            }
        }

        stage("Integration Tests on Staging") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Running integration tests on the staging environment with Selenium" | tee -a ${logFilePath}
                        # Add integration test on staging commands here, for example:
                        # selenium-tests | tee -a ${logFilePath}
                    """
                }
            }
            post {
                success {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Integration Tests on Staging completed successfully. See attached log for details.',
                             subject: 'Integration Tests on Staging: SUCCESS',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
                failure {
                    emailext attachmentsPattern: 'pipeline.log',
                             body: 'The Integration Tests on Staging failed. See attached log for details.',
                             subject: 'Integration Tests on Staging: FAILURE',
                             to: "${env.NOTIFICATION_EMAIL}"
                }
            }
        }

        stage("Deploy to Production") {
            steps {
                script {
                    def logFilePath = env.LOG_FILE
                    sh """
                        echo "Deploying the application to production server using AWS CodeDeploy" | tee -a ${logFilePath}
                        # Add production deployment commands here, for example:
                        # aws deploy push --application-name my-app --s3-location s3://my-bucket/my-app.zip | tee -a ${logFilePath}
                    """
                }
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Pipeline Status: ${currentBuild.result}",
                body: '''<html>
                    <body>
                    <p>Build Status: ${currentBuild.result}</p>
                    <p>Build Number: ${currentBuild.number}</p>
                    <p>Console Output: <a href="${env.BUILD_URL}console">${env.BUILD_URL}console</a></p>
                    </body>
                    </html>''',
                attachmentsPattern: 'pipeline.log',
                to: "${env.NOTIFICATION_EMAIL}",
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html'
            )
        }
    }
}
