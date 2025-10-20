pipeline {
    agent any

    stages {

        stage('Build') {
            echo "==== Build Stage (Java 8) ===="
            steps {
                script {
                    // Use Java 8 for all main build tasks
                    def javaHome8 = tool(name: 'JAVA 8', type: 'jdk')
                    withEnv([
                        "JAVA_HOME=${javaHome8}",
                        "PATH+JAVA=${javaHome8}/bin"
                    ]) {
                        sh 'java -version'
                        sh './gradlew clean build -x test'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            echo "==== SonarQube Analysis Stage (Java 11) ===="
            steps {
                script {
                    // Use Java 11 only for SonarQube
                    def javaHome11 = tool(name: 'JAVA 11', type: 'jdk')
                    withEnv([
                        "JAVA_HOME=${javaHome11}/openjdk-11.0.0.2",
                        "PATH+JAVA=${javaHome11}/openjdk-11.0.0.2/bin"
                    ]) {
                        sh 'java -version'
                        withSonarQubeEnv('dev-sonarcube') {
                            sh './gradlew sonarqube'
                        }
                    }
                }
            }
        }
    }
}
