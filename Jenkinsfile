pipeline {
  agent {
    docker {
      image 'maven:3-alpine'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        dir(path: 'subdir') {
          withCredentials(bindings: [file(credentialsId: 'secretfile', variable: 'FILE')]) {
            sh 'cat $FILE'
          }
          
        }
        
        sh './tools/build.sh'
        ansiColor(colorMapName: 'XTerm') {
          sh '''echo \'1\'
echo $(test1)'''
        }
        
      }
      post {
        success {
          archiveArtifacts 'target/*.hpi,target/*.jpi'
          
        }
        
      }
    }
    stage('Test') {
      steps {
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