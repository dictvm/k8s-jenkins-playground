#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
  containerTemplate(
    name: 'docker',
    image: 'docker/compose:1.9.0',
  )
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  stage ('Pull Secrets')
//  node ('kubernetes') {
//    container('docker') {
      def secrets = [
        [$class: 'VaultSecret', path: 'secret/forecast/password', secretValues: [
          [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey: 'password']
          ]
        ]
      ]
      wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
        sh 'echo $FORECAST_PASSWORD'
      }
//    }
//  }
  
  
  stage ('Build')
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
    } 
  } 
}
