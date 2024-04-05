def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE':'danger'
    ]
pipeline {
    agent any

    tools {
        maven 'M2_HOME'
        jdk 'java11'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/cokke07/controlInventario2.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'mvn clean package'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Test') {
            steps {
                script {
                    bat 'mvn test'
                }
            }
        }
        stage('Sonar Scanner') {
        steps {
            script {
                def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                withCredentials([string(credentialsId: 'sonar', variable: 'sonar')]) {
                    bat "C:/ProgramData/Jenkins/.jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar/bin/sonar-scanner -e -Dsonar.host.url=http://localhost:9000 -Dsonar.login=${sonar} -Dsonar.projectName=mv-maven -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/miapp -Dsonar.tests=src/test/java/miapp -Dsonar.language=java -Dsonar.java.binaries=."
                }
            }
        }
    }

    }
        post {
            always {
                echo 'Slack Notification'
                slackSend channel: '#grupo3',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult} Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} More Info at ${env.BUILD_URL}"
            }
        }
}