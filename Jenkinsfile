pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
    }
    
  }
  stages {
    stage('Test') {
      steps {
         sh 'echo 1'
      }
      post {
        always {
          junit '**/surefire-reports/**/*.xml'
          nunit 'nunit_tests/nunit-tests.xml'
          archiveArtifacts '**/surefire-reports/**/*.xml'
          
        }
        
      }
    }
  }
}
