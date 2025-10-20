pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                script {
                    echo "==== Build Stage (Using Java 8) ===="
                    // Use Java 8 for normal build
                    def javaHome8 = tool(name: 'JAVA 8', type: 'jdk')
                    withEnv([
                        "JAVA_HOME=${javaHome8}",
                        "PATH+JAVA=${javaHome8}/bin"
                    ]) {
                        sh 'echo "Current JAVA_HOME: $JAVA_HOME"'
                        sh 'java -version'
                        
                        // Run your existing build command
                        sh "./build.sh ${stageName}"
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "==== SonarQube Analysis Stage (Using Java 11) ===="
                    // Use Java 11 ONLY for SonarQube analysis
                    def javaHome11 = tool(name: 'JAVA 11', type: 'jdk')
                    withEnv([
                        "JAVA_HOME=${javaHome11}/openjdk-11.0.0.2",
                        "PATH+JAVA=${javaHome11}/openjdk-11.0.0.2/bin"
                    ]) {
                        sh 'echo "Current JAVA_HOME: $JAVA_HOME"'
                        sh 'java -version'

                        withSonarQubeEnv('dev-sonarcube') {
                            sh './gradlew sonarqube'
                        }
                    }
                }
            }
        }

        stage('Release') {
            steps {
                script {
                    echo "==== Release Stage (Using Java 8) ===="
                    def javaHome8 = tool(name: 'JAVA 8', type: 'jdk')
                    withEnv([
                        "JAVA_HOME=${javaHome8}",
                        "PATH+JAVA=${javaHome8}/bin"
                    ]) {
                        sh 'echo "Current JAVA_HOME: $JAVA_HOME"'
                        sh 'java -version'
                        
                        // Your existing deploy script
                        sh "./deploy.sh ${stageName}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo '🎉 Build, Sonar, and Deploy completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check logs above.'
        }
    }
}
