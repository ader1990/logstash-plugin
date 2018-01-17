#!/usr/bin/env groovy

def PowerShellWrapper(psCmd) {
    bat "powershell.exe -NonInteractive -ExecutionPolicy Bypass -Command \"\$ErrorActionPreference='Stop';[Console]::OutputEncoding=[System.Text.Encoding]::UTF8;$psCmd;EXIT \$global:LastExitCode\""
}

pipeline {
    parameters {        
        file(description: 'What environment?', name: 'HELLO', location: "test/myfile.zip")
        string(defaultValue: "TEST", description: 'What environment?', name: 'userFlag')
        // choices are newline separated
        choice(choices: 'US-EAST-1\nUS-WEST-2', description: 'What AWS region?', name: 'region')
    }
    options {
        overrideIndexTriggers(false)
        timeout(time: 1, unit: 'HOURS')
        timestamps()
    }
  agent { 
      node {
      label 'meta_slave'
    }
  }
  stages {
    stage('SStage') {
        steps {
           sh "ls $WORKSPACE"
           sh "cat $HELLO"
        }
    }  
    stage('Parallel Stage') {
            parallel {
                stage('Branch A') {
                    steps {
                        echo env.REGION
                        echo "On Branch A"
                    }
                }
                stage('Branch B') {
                    steps {
                        echo "On Branch B"
                    }
                }
            }
        }
    stage('Build') {
      steps {
          sh 'echo ${BRANCH_NAME}'
          sh "echo ${env.BRANCH_NAME}"
        
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
