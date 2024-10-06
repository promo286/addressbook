pipeline {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '30', numToKeepStr: '2')
    }

    tools {
        maven 'Maven_Home'
    }

    environment {
        BUILD_VERSION = '10'
    }

    stages {
        stage('Git-Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/promo286/addressbook.git']])
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Request Email Approval') {
            steps {
                script {
                    // Send an email notification requesting approval
                    emailext(
                        subject: "Approval Needed: Job '${JOB_NAME} [${BUILD_NUMBER}]' requires your approval",
                        body: """The Jenkins job '${JOB_NAME}' is ready to proceed with the build.

Please approve the job to continue the build process.

Job details:
- Build Number: ${BUILD_NUMBER}
- Build URL: ${BUILD_URL}""",
                        to: 'ravipramoth28@gmail.com'
                    )
                    def userInput = input(
                        id: 'userInput', 
                        message: 'Do you want to proceed with the build?',
                        parameters: [
                            [$class: 'BooleanParameterDefinition', name: 'Approve', defaultValue: true]
                        ]
                    )
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package' 
                sh 'echo "this build ${BUILD_VERSION}"'

            } 
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Job '${JOB_NAME} [${BUILD_NUMBER}]' has completed successfully",
                body: """The Jenkins job '${JOB_NAME}' with build number ${BUILD_NUMBER} has successfully completed.

Check the full details at: ${BUILD_URL}""",
                to: 'ravipramoth28@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Job '${JOB_NAME} [${BUILD_NUMBER}]' has failed",
                body: """The Jenkins job '${JOB_NAME}' with build number ${BUILD_NUMBER} has failed.

Check the full details at: ${BUILD_URL}""",
                to: 'ravipramoth28@gmail.com'
            )
        }
        aborted {
            emailext(
                subject: "ABORTED: Job '${JOB_NAME} [${BUILD_NUMBER}]' was aborted",
                body: """The Jenkins job '${JOB_NAME}' with build number ${BUILD_NUMBER} was aborted.

Check the full details at: ${BUILD_URL}""",
                to: 'ravipramoth28@gmail.com'
            )
        }
    }
}
