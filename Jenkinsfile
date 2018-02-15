#!/usr/bin/env groovy

def commitsList = [];
def commitsString = "";

int i = 0;
int j;
int median = 0;

def didSucceed = true;

node ("ubuntu_kernel_builder") {
    stage ("fetch_commits") {
        def scriptFetch = sh (
                script: "echo 1 2 3 4 5 6 7 8 9 10 11 12 13",
                returnStdout: true
            )
            commitsString = scriptFetch;
            commitsList = commitsString.split(" ");
            j = commitsList.length;
            
            echo "Successfully got commits";
    }
    
    while (i < j) {
        median = (i+j)/2;
        println "Started test with commit: " + commitsList[median];
        def buildResult = build job: 'bisect_random_fail_pipeline', parameters: [string(name: 'RANDOM_VAR', value: commitsList[median] )], propagate: false, wait: true;
        if (buildResult.result == "SUCCESS") {
            i = median + 1;
        } else {
            j = median;
        }
    }
}
