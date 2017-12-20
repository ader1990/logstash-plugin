#!/usr/bin/env groovy

def PowerShellWrapper(psCmd) {
    bat "powershell.exe -NonInteractive -ExecutionPolicy Bypass -Command \"\$ErrorActionPreference='Stop';[Console]::OutputEncoding=[System.Text.Encoding]::UTF8;$psCmd;EXIT \$global:LastExitCode\""
}

pipeline {
    options {
        overrideIndexTriggers(false)
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }
  agent {
    docker {
      image 'maven:3-alpine'
    }
    
  }
  stages {
    stage('Build') {
      steps {
        sh 'echo 124'
        
        ansiColor(colorMapName: 'XTerm') {
          sh '''echo \'1\'
echo $(test1)'''
        }
        
        input(message: 'test', id: '1', ok: '1')
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
