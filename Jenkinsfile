@Library('defra-library@psd-617-secret-scanner') _

node {
    checkout scm
    def dockerImgName = "trufflehog"

    stage("Build truffleHog docker image") {
        sh "docker build . --tag $dockerImgName"
    }

    stage("Run secret scanner") {
        def secretsFound = false

        try {
            secretsFound = secretScanner.scanWithinWindow(dockerImgName, "defra", "ffc-ce", 2)
        } finally {
            if (secretsFound) {
                throw new Exception("Potential secret/s found in scan")
            }
        }
    }
}
