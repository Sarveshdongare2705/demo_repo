pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "==== BUILD STAGE ===="
                sh 'java -version'
                echo ">>> Build steps would go here (like ./gradlew build)"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "==== SONARQUBE ANALYSIS STAGE (Using Java 11) ===="
                script {
                    def javaHome = tool name: 'JAVA 11', type: 'jdk'
                    withEnv([
                        "JAVA_HOME=${javaHome}",
                        "PATH+JAVA=${javaHome}/bin"
                    ]) {
                        echo ">>> Checking Java version in SonarQube stage..."
                        sh 'java -version'

                        withSonarQubeEnv('SonarQubeLocal') {
                            // Make gradlew executable
                            sh 'chmod +x gradlew'
                            echo ">>> Running Gradle Sonar analysis..."
                            sh './gradlew clean build sonarqube -x test -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN'
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "==== DEPLOY STAGE ===="
                sh 'java -version'
                echo ">>> Deployment steps would go here"
            }
        }
    }
}
