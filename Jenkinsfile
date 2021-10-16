pipeline {
    agent any

    tools {
        maven 'Maven'
        nodejs 'NodeJS'
    }
  
    stages {
        stage('initial'){
            steps{
             sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
              '''
            }
        }
        
        stage('Compile'){
            steps{
                sh 'mvn clean compile -e'
            }
        }
        
        stage('Test'){
            steps{
                sh 'mvn clean test -e'
            }
        }
        
        stage('SCA'){
            steps{
                sh 'mvn org.owasp:dependency-check-maven:check'
                
                archiveArtifacts artifacts: 'target/dependency-check-report.html', followSymlinks: false
            }
        }
        
        stage('Sonarqube'){
            steps{
                script{
                    def scannerHome = tool 'SonarQube'
                    
                    withSonarQubeEnv('SonarQube'){
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=devsecops -Dsonar.sources=. -Dsonar.java.binaries=target/classes  -Dsonar.exclusions='**/*/test/**/*, **/*/acceptance-test/**/*, **/*.html' -Dsonar.host.url=http://192.168.70.146:9001 -Dsonar.login=4cb8af5218a018b94fb22a8b49889c8464e921a6"
                    }
                }
            }
        }
        
    }
    
}
