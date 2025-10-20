pipeline {
    agent any

    stages {
        stage('SonarQube Analysis') {
            steps {
                echo "==== SonarQube Analysis Stage (Using Java 11) ===="
                withEnv([
                    "JAVA_HOME=${tool name: 'JAVA 11', type: 'jdk'}",
                    "PATH+JAVA=${tool name: 'JAVA 11', type: 'jdk'}/jdk-11.0.0.2/bin"
                ]) {
                    sh 'echo "Java Home: $JAVA_HOME"'
                    sh 'java -version' // Verify Java 11 is used
                    withSonarQubeEnv('SonarQubeLocal') {
                        sh 'chmod +x gradlew'
                        sh './gradlew clean build sonarqube -x test'
                    }
                }
            }
        }
    }
}
