pipeline {
    agent any

    stages {
        stage('Build') {
    steps {
        echo '==== Build Stage (Using Java 8) ===='
        withEnv([
            "JAVA_HOME=${tool name: 'JAVA 8', type: 'jdk'}",
            "PATH+JAVA=${tool name: 'JAVA 8', type: 'jdk'}/bin"
        ]) {
            sh 'java -version'
            sh 'chmod +x gradlew'
            sh './gradlew clean build -x test'
        }
    }
}


        stage('SonarQube Analysis') {
            steps {
                echo '==== SonarQube Analysis Stage (Using Java 11) ===='
                withEnv([
                    "JAVA_HOME=${tool name: 'JAVA 11', type: 'jdk'}",
                    "PATH+JAVA=${tool name: 'JAVA 11', type: 'jdk'}/bin"
                ]) {
                    sh 'java -version'
                    withSonarQubeEnv('dev-sonarcube') {
                        sh './gradlew sonarqube'
                    }
                }
            }
        }

        stage('Release') {
            steps {
                echo '==== Release Stage ===='
                // your existing release logic here
            }
        }
    }

    post {
        success {
            echo '🎉 Build + Sonar completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check logs.'
        }
    }
}
