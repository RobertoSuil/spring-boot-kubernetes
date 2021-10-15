pipeline {
    agent any

   tools {
       maven "Maven"
       nodejs "NodeJS"

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
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devsecops -Dsonar.sources=target/ -Dsonar.host.url=http://192.168.70.142:9001 -Dsonar.login=38cd56a2546c29ba862e0554e5c9fda9a9104a57"                
                    }
                }
           }
        }
    }
}
