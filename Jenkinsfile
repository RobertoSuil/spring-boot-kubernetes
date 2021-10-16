pipeline {
  agent any

  tools {
    maven 'mvn-3.8.3'
  }

  stages {
    stage('Build') {
      steps {
        withMaven(maven : 'mvn-3.8.3') {
          sh "mvn package"
        }
      }
    }

    stage ('OWASP Dependency-Check Vulnerabilities') {
      steps {
        withMaven(maven : 'mvn-3.8.3') {
          sh 'mvn dependency-check:check'
        }

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
