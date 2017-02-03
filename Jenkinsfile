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
  agent container('slavebuild') { 
    stages { 
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
         node ('slavebuild') {
           container('docker') {
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
          } // container
        } // node
      } // stage
    } // stages
  } // agent
} // pipeline
