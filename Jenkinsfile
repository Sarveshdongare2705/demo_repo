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
            environment {
                JAVA_HOME = tool(name: 'jdk11', type: 'jdk')
                PATH = "${JAVA_HOME}/bin:${env.PATH}"
            }
            steps {
                echo "==== SonarQube Analysis Stage ===="
                sh 'echo Switching to Java 11 for SonarQube'
                sh 'echo Current Java version:'
                sh 'java -version'
                withSonarQubeEnv('SonarQubeLocal') {
                    // Double check which java is being used by Gradle itself
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
