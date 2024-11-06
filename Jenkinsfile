pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Command to build the project
                sh './gradlew assemble'
            }
        }
        stage('Test') {
            steps {
                // Command to run tests
                sh './gradlew test'
            }
        }
    }
    post {
        always {
            // Clean up after the build
            cleanWs()
        }
    }
}
