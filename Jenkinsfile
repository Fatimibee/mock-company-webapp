pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Replace nohup with an alternative for Windows
                    if (isUnix()) {
                        sh 'nohup ./build.sh &'
                    } else {
                        bat 'start /B build.bat'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh './run_tests.sh'
                    } else {
                        bat 'run_tests.bat'
                    }
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                junit '**/target/test-*.xml'  // Adjust the test results path as needed
            }
        }

        stage('Post Actions') {
            steps {
                cleanWs()  // Clean workspace after build
            }
        }
    }

    post {
        failure {
            echo 'Build or tests failed!'
        }
    }
}
