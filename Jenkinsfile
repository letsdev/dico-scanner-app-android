#!/usr/bin/env groovy

pipeline {
    agent {
        docker {
            label 'docker'
            image "gitlab-registry.letsdev.intern/docker/android:1.7.0"
            registryCredentialsId 'gitlab-jenkins-user-credentials'
            registryUrl "https://gitlab-registry.letsdev.intern"
            reuseNode true
            //noinspection GroovyAssignabilityCheck
            args "--network=host"
        }
    }
    options {
        timestamps()
        ansiColor('xterm')
        buildDiscarder logRotator(numToKeepStr: '20')
        disableConcurrentBuilds()
    }
    stages {
        stage('Build') {
            steps {
                sh "./gradlew --no-daemon clean -q"
                sh "./gradlew --no-daemon bundleRelease -q"
            }
            post {
                success {
                    archiveArtifacts artifacts: '**/build/outputs/bundle/**/*.aab',
                            fingerprint: true,
                            onlyIfSuccessful: true,
                            allowEmptyArchive: true

                    archiveArtifacts artifacts: '**/build/outputs/mapping/**/*.txt',
                            fingerprint: true,
                            onlyIfSuccessful: true,
                            allowEmptyArchive: true
                }
            }
        }
    }
}
