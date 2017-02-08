#!/usr/bin/env groovy
// vim: set ft=groovy:

def app_name = 'testing.daniel.k8s.ivx.cloud'

podTemplate(label: 'kubernetes', containers: [
  containerTemplate(
    name: 'docker',
    image: 'docker/compose:1.10.0',
    ttyEnabled: true,
    command: 'cat'
  )
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  node {
    // get secrets from vault
    def secrets = [
      [$class: 'VaultSecret', path: 'secret/forecast/auth', secretValues: [
        [$class: 'VaultSecretValue', envVar: 'FORECAST_USER', vaultKey: 'user'],
        [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:  'password']]
      ]
    ]
    
    stage ('Show Secrets') {
      // secrets should be available
      wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
        sh 'echo $FORECAST_USER'
        sh 'echo $FORECAST_PASSWORD'
      }
    }
    
    stage ('Checkout example') {
      container('docker') {
        git 'https://github.com/digitalocean/netbox.git'
      }
    }
  
    stage ('Build something') {
      container('docker') {
        try {
          sh 'docker-compose build --pull'
          sh 'docker-compose up -d'
        } catch(err) {
          sh 'docker-compose down -v --remove-orphans'
          throw err
        } finally {
          // print secrets again
          wrap([$class: 'VaultBuildWrapper', vaultSecrets: secrets]) {
            sh "docker-compose down -v --remove-orphans"
            sh 'echo $FORECAST_USER'
            sh 'echo $FORECAST_PASSWORD'
          }
        }
      }
    }
  }
}
