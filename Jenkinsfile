pipeline {
    agent any

    tools {
        jdk 'jdk8' // default JDK for all stages
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
        booleanParam(name: 'RUN_SONAR', defaultValue: true, description: 'Run SonarQube analysis?')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Deployment environment')
    }

    stages {
        stage('Checkout') {
            steps {
                echo "==== Checkout Stage ===="
                sh 'echo "Current Java version:" && java -version'
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/Sarveshdongare2705/demo_repo.git'
            }
        }

        stage('Build') {
            steps {
                echo "==== Build Stage ===="
                sh 'echo "Current Java version:" && java -version'
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression { params.RUN_SONAR }
            }
            environment {
                JAVA_HOME = tool(name: 'jdk11', type: 'jdk')
                PATH = "${JAVA_HOME}/bin:${env.PATH}"
            }
            steps {
                echo "==== SonarQube Analysis Stage ===="
                echo "Switching to Java 11 for SonarQube"
                sh 'echo "Current Java version:" && java -version'
                withSonarQubeEnv('SonarQubeLocal') {
                    sh './gradlew sonarqube'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "==== Deploy Stage ===="
                sh 'echo "Current Java version:" && java -version'
                echo "Deploying to ${params.ENVIRONMENT}"
            }
        }
    }

    post {
        always {
            echo "==== Pipeline Completed ===="
            sh 'echo "Final Java version:" && java -version'
        }
    }
}
