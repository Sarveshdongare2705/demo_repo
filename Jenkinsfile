pipeline {
    agent any

    stages {
        stage('SonarQube Analysis') {
            steps {
                echo "==== SonarQube Analysis Stage (Using Java 11) ===="
                withEnv([
                    "JAVA_HOME=${tool name: 'JAVA 11', type: 'jdk'}",
                    "PATH+JAVA=${tool name: 'JAVA 11', type: 'jdk'}/bin"
                ]) {
                    sh 'java -version'   // Verify Java 11 is being used
                    withSonarQubeEnv('SonarQubeLocal') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
    }
}
