pipeline {
    agent {
        docker {
            image 'okteto/maven:3'
        }
    }

    tools {
        // Specify the exact Maven version you want to use
        maven 'M2_HOME'
    }

   
    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from SCM (e.g., Git)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    try {
                        // Clean, install, and package your Maven project
                        sh 'mvn clean install package'
                    } catch (Exception e) {
                        // Handle build failures
                        currentBuild.result = 'FAILURE'
                        error("Build failed: ${e.message}")
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    try {
                        // Run tests
                        sh 'mvn test'
                    } catch (Exception e) {
                        // Handle test failures
                        currentBuild.result = 'FAILURE'
                        error("Tests failed: ${e.message}")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy'
                // Add deployment steps here if needed
            }
        }
    }

    post {
        always {
            // Archive build artifacts, publish reports, or perform cleanup tasks
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
