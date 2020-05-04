# FFC git secret scanning

## Overview

Utilities for configuring and running secret scanning of FFC git repositories using the functions defined in the [FFC Jenkins pipeline library](https://github.com/DEFRA/ffc-jenkins-pipeline-library) that utilise the [truffleHog](https://github.com/dxa4481/truffleHog) tool. It provides the following:

* A `Jenkinsfile` to set up regular sliding window scanning of FFC repos
* A `Dockerfile` for building a Docker image to run `truffleHog` with an `exclude-patterns.txt` file
* File path and secret string excludes for the `truffleHog` scan to avoid false positives
* Additional helpful utilities

## Adding files and file paths to exclude from the scan

To add additional files or file paths to be excluded from the `truffleHog` scanning, add additional entries to the `exclude-patterns.txt` file that gets built into the docker image. Please refer to the [truffleHog](https://github.com/dxa4481/truffleHog) page for formatting of the `exclude-patterns.txt` file.

**WARNING** Any entries added to the `exclude-patterns.txt` file will be excluded from the scanning of all repositories. There is currently no fine-grained way of excluding paths from individual repositories.

## Adding strings to exclude from the scan

To add additional strings to be excluded from the `truffleHog` scanning, add additional entries to the `exclude-strings.txt` file that gets passed to the functions to run `truffleHog` defined in the [FFC Jenkins pipeline library](https://github.com/DEFRA/ffc-jenkins-pipeline-library). These strings would typically represent recurring false positives.

**WARNING** Any entries added to the `exclude-strings.txt` file will be excluded from the scanning of all repositories. There is currently no fine-grained way of excluding strings from individual repositories.

## Using the truffleHog docker image locally

Build the docker image:

```
docker build . --tag trufflehog
```

Run the docker image:

```
docker run trufflehog <URL_TO_GIT_REPO>
```

For the `truffleHog` help message just run `docker run trufflehog`. Please refer to the [truffleHog](https://github.com/dxa4481/truffleHog) page for additional details on run-time options.
