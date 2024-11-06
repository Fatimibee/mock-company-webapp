pipeline {
    agent any

    environment {
        // Optional: Specify Gradle home if necessary
        GRADLE_HOME = '/path/to/gradle'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the project using Gradle wrapper
                    sh './gradlew assemble'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run the tests using Gradle wrapper
                    sh './gradlew test'
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                // Archive test results in XML format, adjust path if necessary
                archiveArtifacts allowEmptyArchive: true, artifacts: '**/build/test-results/test/*.xml', onlyIfSuccessful: true
                junit '**/build/test-results/test/*.xml' // Optional: To publish test results in Jenkins
            }
        }
    }

    post {
        always {
            // Clean workspace after the build/test
            cleanWs()
        }
        success {
            // Optional: Success message
            echo "Build and tests passed successfully!"
        }
        failure {
            // Optional: Failure message
            echo "Build or tests failed!"
        }
    }
}
