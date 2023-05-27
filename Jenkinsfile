pipeline{
    agent any

    tools{
    maven 'jenkinsmaven'
    }
    stages {
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
        stage('Deploy'){
            steps{
                echo 'Desplegando el area de desarrollo'
            }
        }
    }
}