pipeline {
    agent any

    tools {
        gradle 'gradle-8'  // <-- name must match the Gradle tool you configured in Jenkins
        jdk 'jdk17'         // <-- name must match your JDK tool config
    }

    environment {
        SONARQUBE_ENV = credentials('sonarqube-token') // Jenkins credential ID
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('local-sonar') {
                    sh """
                        ./gradlew sonarqube \
                            -Dsonar.projectKey=demo \
                            -Dsonar.projectName=demo \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=${SONARQUBE_ENV}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
