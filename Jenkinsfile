pipeline {
    agent any

    stages {
        stage('SonarQube Analysis') {
            steps {
                echo '==== SonarQube Analysis Stage (Using Java 11) ===='
                sh 'java -version'
                withSonarQubeEnv('SonarQubeLocal') {
                sh 'chmod +x gradlew'
                sh './gradlew sonarqube'
                }
            }
        }
}
}