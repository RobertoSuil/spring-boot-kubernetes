pipeline {
    agent any

   tools {
        maven 'Maven'
    }

    stages {

        stage ('Initial') {
            steps {
              echo '========================================='
              echo '                Inicializando '
              echo '========================================='
              sh '''
                   echo "PATH = ${PATH}"
                   echo "M2_HOME = ${M2_HOME}"
               '''
            }
        }
        stage ('Compile') {
            steps {
                echo '========================================='
                echo '                Compilando '
                echo '========================================='
                 sh 'mvn clean compile -e'
            }
        }
        stage ('Test') {
            steps {
                echo '========================================='
                echo '                Testeando '
                echo '========================================='
                sh 'mvn clean test -e'
            }
        }

        
        stage('SonarQube analysis - SAST') {
           steps{
               echo '========================================='
              echo '                SONARQUBE '
              echo '========================================='
                script {
                    def scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv('SonarQube') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=tarea4-devsecops -Dsonar.sources=target/ -Dsonar.host.url=http://192.168.8.112:9000 -Dsonar.login=220d8b11c0d69a6137dae715e362b72d711a2a9e"
                    }
                }
           }
        }
    }
}
