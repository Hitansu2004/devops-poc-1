pipeline {
    agent any
    environment {
        // Build number tagging
        BUILD_TAG = "build-${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                // Jenkins automatically checks out the source code if configured as Pipeline from SCM.
                // Since this might run as a local directory pipeline, we'll try checkout scm
                // In some setups, you might need an explicit git url instead.
                checkout scm
                echo "Source code checked out successfully."
            }
        }
        stage('Test') {
            steps {
                // Run Unit Tests
                echo "Testing Java App..."
                sh '/usr/local/bin/mvn clean test'
            }
        }
        stage('Package') {
            steps {
                // Package the Maven app into a JAR file (skipping tests as they just ran)
                echo "Packaging Java App..."
                sh '/usr/local/bin/mvn package -DskipTests'
            }
        }
        stage('Archive Artifacts') {
            steps {
                // Archive both the Java backend JAR and the frontend files
                echo "Archiving artifacts (target/*.jar and frontend/**/*)..."
                archiveArtifacts artifacts: 'target/*.jar, frontend/**/*', allowEmptyArchive: false
            }
        }
    }
    post {
        failure {
            // Email notification on failure (Commented out for local POC as SMTP is not configured)
            echo "Pipeline failed! (Skipping email as SMTP port 25 is not configured locally)"
            // mail to: 'devops-team@example.com',
            //      subject: "Build Failed: ${currentBuild.fullDisplayName}",
            //      body: "Build failed for POC 1. Please check the logs at ${env.BUILD_URL}"
        }
        success {
            echo "Pipeline succeeded! Build Tag: ${BUILD_TAG}"
        }
    }
}
