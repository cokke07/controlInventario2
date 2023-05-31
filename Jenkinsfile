pipeline{
    agent any

    tools{
        maven 'jenkinsmaven'
    }
    stages {
            stage('Checkout') {
                steps {
                    git branch: 'main', url: 'https://github.com/cokke07/controlInventario2.git'
                }
            }
            stage('Build'){
                steps{
                    echo 'Construyendo la aplicacion'
                    sh 'mvn clean package'
                    }
                }
            stage('Archivar artefacto'){
                steps{
                    archiveArtifacts 'target/controlInventario2-0.0.1-SNAPSHOT.jar'
                    }
            }
            stage('Test'){
                steps{
                    echo 'Ejecutar los test'
                    sh 'mvn test'
                }
            }
            stage('Sonar Scanner') {
                steps {
                    script {
                        def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://SonarQube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=mv-maven -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/miapp -Dsonar.tests=src/test/java/miapp -Dsonar.language=java -Dsonar.java.binaries=."
                        }
                    }
                }
            }
            stage('Deploy'){
                steps{
                    echo 'Desplegando el area de desarrollo'
                }
            }
        }
    }