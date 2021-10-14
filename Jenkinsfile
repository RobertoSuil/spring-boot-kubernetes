pipeline {
    agent any

   tools {
       maven 'Maven'
    }

    stages {

        stage ('Initial') {
            steps {
              echo '========================================='
              echo '                Inicializando... '
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
                echo '                Compilando... '
                echo '========================================='
                 sh 'mvn clean compile -e'
            }
        }
        stage ('Test') {
            steps {
                echo '========================================='
                echo '                Testeando... '
                echo '========================================='
                sh 'mvn clean test -e'
            }
        }
        
     
        stage('SonarQube - SAST') {
           steps{
               echo '========================================='
              echo '                SonarQube... '
              echo '========================================='
                script {
                    def scannerHome = tool name: 'sonar_scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
                    withSonarQubeEnv('sonar_scanner') { 
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=tarea4-devsecops -Dsonar.sources=target/ -Dsonar.host.url=http://192.168.70.143:9000 -Dsonar.login=7d0f5449a0efb11571604db587cb222fea969ebc"
                    }
                }
           }
        }
    }
}
