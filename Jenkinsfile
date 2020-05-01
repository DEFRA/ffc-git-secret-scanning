@Library('defra-library@psd-617-secret-scanner') _

node {
    properties([pipelineTriggers([cron('10 * * * *')])])

    checkout scm
    def dockerImgName = "trufflehog"
    def currentHour = new Date().format("HH")

    stage("Build truffleHog docker image") {
        sh "docker build . --tag $dockerImgName"
    }

    stage("Run secret scanner") {
        def secretsFound = false

        try {
            if (currentHour == "13") {
                echo "Running daily scan"
                secretsFound = secretScanner.scanWithinWindow(dockerImgName, "defra", "ffc", 72)
            }
            else {
                echo "Running hourly scan"
                secretsFound = secretScanner.scanWithinWindow(dockerImgName, "defra", "ffc", 2)
            }
        } finally {
            if (secretsFound) {
                throw new Exception("Potential secret/s found in scan")
            }
        }
    }
}
