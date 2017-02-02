#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    [name: 'docker', image: 'docker/compose:1.9.0', ttyEnabled: true, command: 'cat']
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  stage 'Vaulttest'
    node {
      def secrets = [
          [$class: 'VaultSecret', path: 'secret/forecast/password', secretValues: [
              [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:
              'password']]]
      ]
    
      wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
          sh 'echo $FORECAST_PASSWORD'
      }
    }

    node ('kubernetes') {
      container('docker') {
        try {
          git 'https://github.com/digitalocean/netbox.git'
          sh 'docker-compose build --pull'
          sh 'docker-compose up -d'
          sh 'echo $FORECAST_PASSWORD'
        } catch(err) {
          sh 'docker-compose down -v --remove-orphans'
          throw err
        } finally {
          sh 'docker-compose down -v --remove-orphans'
        }
      } // container
    } // node


} // end
