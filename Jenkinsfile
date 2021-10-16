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
        stage ('OWASP Dependency-Check Vulnerabilities') {  
            steps {  
                echo '========================================='
                echo '                Dependency-Check '
                echo '========================================='
                sh 'mvn dependency-check:check' 
                dependencyCheckPublisher pattern: 'target/dependency-check-report.xml'  
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
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devsecops -Dsonar.sources=target/ -Dsonar.host.url=http://192.168.70.146:9001 -Dsonar.login=4cb8af5218a018b94fb22a8b49889c8464e921a6"                
                    }
                }
           }
        }
    }
}
