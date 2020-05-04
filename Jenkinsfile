@Library('defra-library@4') _

node {
    properties([pipelineTriggers([cron('0 */2 * * *')])])

    checkout scm

    def dockerImgName = "trufflehog"
    def currentHour = new Date().format("HH")
    def excludeStrings = []

    stage("Build truffleHog docker image") {
        sh "docker build . --tag $dockerImgName"
    }

    stage("Read exclude strings file") {
        def fileContent = readFile "exclude-strings.txt"
        fileContent.split().each { excludeStrings.add(it) }

        echo "EXCLUDE STRINGS = $excludeStrings"
    }

    stage("Run secret scanner") {
        def secretsFound = false

        try {
            if (currentHour == "02") {
                echo "*** RUNNING DAILY SCAN ***"
                secretsFound = secretScanner.scanWithinWindow(
                    'github-auth-token',
                    dockerImgName,
                    "defra",
                    "ffc",
                    24,
                    excludeStrings,
                    "#secretdetection"
                )
            }
            else {
                echo "*** RUNNING HOURLY SCAN ***"
                secretsFound = secretScanner.scanWithinWindow(
                    'github-auth-token',
                    dockerImgName,
                    "defra",
                    "ffc",
                    2,
                    excludeStrings,
                    "#secretdetection"
                )
            }
        } finally {
            if (secretsFound) {
                throw new Exception("Potential secret/s found in scan")
            }
        }
    }
}
