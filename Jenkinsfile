pipeline {
    agent any

    tools {
        gradle 'gradle-8'   // must match name in Global Tool Config
        jdk 'jdk17'         // or 'jdk17' if you later add it
    }

    stages {
        stage('Checkout') {
    steps {
        git branch: 'main', url: 'https://github.com/Sarveshdongare2705/demo_repo.git'
    }
}

        stage('Build') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean build'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeLocal') {  // name must match the SonarQube server ID in Jenkins
                    sh './gradlew sonarqube'
                }
            }
        }
    }
}
