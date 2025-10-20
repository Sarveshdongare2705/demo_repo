pipeline {
    agent any

    stages {
        stage('SonarQube Analysis') {
            steps {
                echo "==== SonarQube Analysis Stage (Using Java 11) ===="
                script {
                    def javaHome = tool name: 'JAVA 11', type: 'jdk'
                    def actualJavaHome = "${javaHome}/jdk-11.0.0.2"
                    withEnv([
                        "JAVA_HOME=${actualJavaHome}",
                        "PATH+JAVA=${actualJavaHome}/bin"
                    ]) {
                        sh 'echo "Java Home: $JAVA_HOME"'
                        sh 'java -version' // confirm correct Java
                        withSonarQubeEnv('SonarQubeLocal') {
    sh 'echo "SONAR_HOST_URL=$SONAR_HOST_URL"'
    sh 'echo "SONAR_AUTH_TOKEN=$SONAR_AUTH_TOKEN"'
    sh 'chmod +x gradlew'
    sh './gradlew clean build sonarqube -x test -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN'
}

                    }
                }
            }
        }
    }
}
