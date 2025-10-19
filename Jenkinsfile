pipeline {
    agent any

    tools {
        jdk 'jdk8' // Default JDK for build etc.
    }

    stages {
        stage('Checkout') {
            steps {
                echo "==== Checkout Stage ===="
                sh 'echo Current Java version:'
                sh 'java -version'
                git branch: 'main', url: 'https://github.com/Sarveshdongare2705/demo_repo.git'
            }
        }

        stage('Build') {
            steps {
                echo "==== Build Stage ===="
                sh 'echo Current Java version:'
                sh 'java -version'
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            tools {
                jdk 'jdk11' // Switch to Java 11 for this stage
            }
            steps {
                echo "==== SonarQube Analysis Stage ===="
                sh 'echo Switching to Java 11 for SonarQube'
                sh 'echo Current Java version:'
                sh 'java -version'
                withSonarQubeEnv('SonarQubeLocal') {
                    // Verify Gradle is now using Java 11
                    sh './gradlew --version'
                    sh './gradlew sonarqube --info'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "==== Deploy Stage ===="
                sh 'echo Current Java version:'
                sh 'java -version'
            }
        }
    }

    post {
        always {
            echo "==== Pipeline Completed ===="
            sh 'echo Final Java version:'
            sh 'java -version'
        }
    }
}