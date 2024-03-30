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