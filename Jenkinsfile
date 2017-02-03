#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'slavebuild', containers: [
    containerTemplate(
        name: 'docker',
        image: 'docker/compose:1.9.0',
    )
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')])

pipeline {
    agent any
        stages { 
            node ('slavebuild') {
            container('docker') {
            stage 'Gather Secrets' {
                def secrets = [
                    [$class: 'VaultSecret', path: 'secret/forecast/password', secretValues: [
                        [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:
                            'password']
                        ]
                    ]
                ]
                wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
                    sh 'echo $FORECAST_PASSWORD'
                }
            }
            stage 'Build Image' {
                steps {
                    try {
                        git 'https://github.com/digitalocean/netbox.git'
                        sh 'docker-compose build --pull'
                        sh 'docker-compose up -d'
                    } catch(err) {
                        sh 'docker-compose down -v --remove-orphans'
                        throw err
                    } finally {
                        sh """
                        echo $FORECAST_PASSWORD
                        docker-compose down -v --remove-orphans
                        """
                    }
                 } // steps
            } // stage
        } // container
    } // node
} // stages
} // pipeline
