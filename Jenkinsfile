#!/usr/bin/env groovy

def PowerShellWrapper(psCmd) {
    bat "powershell.exe -NonInteractive -ExecutionPolicy Bypass -Command \"\$ErrorActionPreference='Stop';[Console]::OutputEncoding=[System.Text.Encoding]::UTF8;$psCmd;EXIT \$global:LastExitCode\""
}

pipeline {
    parameters {        
        string(defaultValue: "TEST", description: 'What environment?\n http://hello \n Hello', name: 'userFlag')
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
  
  stages{
      stage('validation') {
       parallel {
    stage('SStage') {
        agent{
      node {
      label 'hyper-v'
    }
    }
        steps {
           powershell "ls $WORKSPACE"
            powershell '''
            $max=50
            while ($max -gt 0) {
                $max = $max -1
                Start-Sleep 1
                Write-Output (Get-Date)
            }
            '''
           
        }
    }
           
           stage('SStage1') {
        agent{
      node {
      label 'hyper-v'
    }
    }
        steps {
           powershell "ls $WORKSPACE"
            powershell '''
            $max=10
            while ($max -gt 0) {
                $max = $max -1
                Start-Sleep 1
                Write-Host (Get-Date)
            }
            '''
           
        }
           }}
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
