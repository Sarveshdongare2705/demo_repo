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
                withSonarQubeEnv('local-sonar') {  // name must match the SonarQube server ID in Jenkins
                    sh """
                        ./gradlew sonarqube \
                          -Dsonar.projectKey=demo \
                          -Dsonar.projectName=demo \
                          -Dsonar.host.url=http://localhost:9000
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
