pipeline {
    agent any

    options {
        buildDiscarder logRotator(
            artifactDaysToKeepStr: '', 
            artifactNumToKeepStr: '', 
            daysToKeepStr: '30', 
            numToKeepStr: '2'
        )
    }

    tools {
        maven 'Maven'
    }

    stages {
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
                sh 'echo "this build ${BUILD_NUMBER}"'
            }
        }
    }
}
