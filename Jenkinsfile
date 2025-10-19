pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
        booleanParam(name: 'RUN_SONAR', defaultValue: true, description: 'Run SonarQube analysis?')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'qa', 'prod'], description: 'Deployment environment')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/Sarveshdongare2705/demo_repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x gradlew0'
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression { params.RUN_SONAR }
            }
            steps {
                withSonarQubeEnv('SonarQubeLocal') {
                    sh './gradlew sonar'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying to ${params.ENVIRONMENT}"
            }
        }
    }
}
