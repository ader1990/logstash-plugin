pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh './tools/build.sh'
      }
      post {
        success {
          archiveArtifacts 'target/*.hpi,target/*.jpi'
          
        }
        
      }
    }
    stage('Test') {      
      steps {dir('subdir') {
        withCredentials([file(credentialsId: 'secretfile', variable: 'FILE')]) {
          sh 'cat $FILE'
        }
      }
        sh './tools/test.sh'
      }
      post {
        always {
          junit '**/surefire-reports/**/*.xml'
          nunit(testResultsPattern: 'nunit_tests/nunit-tests.xml')
          
        }
        
      }
    }
  }
}
