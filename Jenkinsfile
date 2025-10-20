def gitBranch = env.BRANCH_NAME ?: 'main'
def gitTag = env.TAG ? "refs/tags/${env.TAG}" : null
def gitRef = gitTag ? gitTag : gitBranch
def buildVersion = gitTag ? gitTag : 'snapshot'

echo "branch = ${gitBranch}"
echo "tag = ${gitTag}"
echo "ref = ${gitRef}"
echo "buildVersion = ${buildVersion}"

node {
    properties([
        disableConcurrentBuilds(),
        parameters([
            string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build'),
            string(name: 'TAG', defaultValue: '', description: 'Tag to build (optional)')
        ])
    ])
    
    echo sh(returnStdout: true, script: 'env')
    
    try {
        stage('Checkout') {
            echo "==== Checkout Stage ===="
            echo "Checking out git ref: ${gitRef}"
            
            sh 'echo Current Java version:'
            sh 'java -version'
            
            checkout([
                $class: 'GitSCM',
                branches: [[name: "${gitRef}"]],
                doGenerateSubmoduleConfigurations: false,
                extensions: [],
                submoduleCfg: [],
                userRemoteConfigs: [[
                    url: 'https://github.com/Sarveshdongare2705/demo_repo.git'
                ]]
            ])
        }
        
        stage('Build') {
            echo "==== Build Stage ===="
            sh 'echo Current Java version:'
            sh 'java -version'
            sh 'chmod +x gradlew'
            sh './gradlew clean build'
        }
        
        stage('SonarQube Analysis') {
            echo "==== SonarQube Analysis Stage ===="
            withEnv([
                "JAVA_HOME=${tool name: 'jdk11', type: 'jdk'}",
                "PATH+JAVA=${tool name: 'jdk11', type: 'jdk'}/bin"
            ]) {
                sh 'echo Switching to Java 11 for SonarQube'
                sh 'echo Current Java version:'
                sh 'java -version'
                
                withSonarQubeEnv('SonarQubeLocal') {
                    sh './gradlew --version'
                    sh './gradlew sonarqube --info'
                }
            }
        }
        
        stage('Deploy') {
            echo "==== Deploy Stage ===="
            sh 'echo Current Java version:'
            sh 'java -version'
        }
        
    } catch (Exception e) {
        currentBuild.result = 'FAILURE'
        throw e
    } finally {
        echo "==== Pipeline Completed ===="
        sh 'echo Final Java version:'
        sh 'java -version'
    }
}