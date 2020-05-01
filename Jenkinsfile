@Library('defra-library@psd-617-secret-scanner') _

node {
    checkout scm
    def dockerImg

    stage("Build truffleHog docker image") {
        dockerImg = docker.build "trufflehog"
        dockerImg.run()
    }

    // stage("Run secret scanner") {
    //     def secretsFound = false

    //     try {
    //         secretsFound = secretScanner.scanWithinWindow('defra', 'ffc-ce', 2)
    //     } finally {
    //         if (secretsFound) {
    //             throw new Exception("Potential secret/s found in scan")
    //         }
    //     }
    // }
}
