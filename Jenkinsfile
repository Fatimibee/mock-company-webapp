pipeline {
    agent any
    environment {
        // Optional: Caching Gradle dependencies for faster builds (adjust path as needed)
        GRADLE_USER_HOME = '/path/to/cache/.gradle'
    }
    parameters {
        // Optional: Allows you to specify the deployment environment
        string(name: 'DEPLOY_ENV', defaultValue: 'dev', description: 'Deployment environment')
    }
    stages {
        stage('Build') {
            steps {
                // Command to build the project (you can change 'sh' to 'bat' for Windows)
                sh './gradlew assemble'
            }
        }
        stage('Lint') {
            steps {
                // Command to run static code analysis (e.g., Checkstyle)
                sh './gradlew checkstyleMain'
            }
        }
        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        // Command to run unit tests
                        sh './gradlew test'
                    }
                }
                stage('Integration Tests') {
                    steps {
                        // Command to run integration tests (if applicable)
                        sh './gradlew integrationTest'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                // Optional: Deploy to the environment based on parameter (e.g., dev, prod)
                sh "./deploy.sh ${params.DEPLOY_ENV}"
            }
        }
    }
    post {
        success {
            // Archive build artifacts (e.g., JAR or WAR file)
            archiveArtifacts 'build/libs/*.jar'
            // Notify on Slack if the build is successful
            slackSend(channel: '#build-notifications', message: "Build succeeded!")
        }
        failure {
            // Notify on Slack if the build fails
            slackSend(channel: '#build-notifications', message: "Build failed!")
        }
        always {
            // Clean up after the build
            cleanWs()
        }
    }
}
