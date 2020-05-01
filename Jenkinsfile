@Library('defra-library@psd-617-secret-scanner') _

node {
    properties([pipelineTriggers([cron('0 * * * *')])])

    checkout scm

    def dockerImgName = "trufflehog"
    def currentHour = new Date().format("HH")

    stage("Build truffleHog docker image") {
        sh "docker build . --tag $dockerImgName"
    }

    stage("Run secret scanner") {
        def secretsFound = false

        try {
            if (currentHour == "23") {
                echo "*** RUNNING DAILY SCAN ***"
                secretsFound = secretScanner.scanWithinWindow('github-auth-token', dockerImgName, "defra", "ffc", 72, "#secretdetection")
            }
            else {
                echo "*** RUNNING HOURLY SCAN ***"
                secretsFound = secretScanner.scanWithinWindow('github-auth-token', dockerImgName, "defra", "ffc", 2, "#secretdetection")
            }
        } finally {
            if (secretsFound) {
                throw new Exception("Potential secret/s found in scan")
            }
        }
    }
}
