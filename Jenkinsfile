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
    stage ('Gather Secrets') {
      def fc_secrets = [
        [$class: 'VaultSecret', path: 'secret/forecast/auth',
          secretValues: [
            [$class: 'VaultSecretValue', envVar: 'FORECAST_USER', vaultKey: 'user'],
            [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:  'password']
          ]
        ]
      ]
      wrap([$class: 'VaultBuildWrapper', vaultSecrets: fc_secrets]) {
        sh 'echo $FORECAST_USER'
        sh 'echo $FORECAST_PASSWORD'
      }
    }
    
    stage ('Checkout') {
      container('docker') {
        git 'https://github.com/digitalocean/netbox.git'
      }
    }
  
    stage ('Build') {
      container('docker') {
        try {
          wrap([$class: 'VaultBuildWrapper', vaultSecrets: fc_secrets]) {
            sh 'echo $FORECAST_USER'
            sh 'echo $FORECAST_PASSWORD'
          }
          sh 'docker-compose build --pull'
          sh 'docker-compose up -d'
        } catch(err) {
          sh 'docker-compose down -v --remove-orphans'
          throw err
        } finally {
          sh "docker-compose down -v --remove-orphans"
        }
      }
    }
  }
}
