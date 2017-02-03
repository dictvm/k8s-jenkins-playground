#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    containerTemplate(
        name: 'docker',
        image: 'docker/compose:1.9.0',
        envVar: 'FORECAST_PASSWORD',
    )
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  stage 'Vaulttest'
     node ('kubernetes') {
      container('docker') {
      def secrets = [
          [$class: 'VaultSecret', path: 'secret/forecast/password', secretValues: [
              [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:
              'password']]]
      ]
      wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
          sh 'echo $FORECAST_PASSWORD for ${env.BUILD_NUMBER}'
      }
    }

     node ('kubernetes') {
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

} // end
