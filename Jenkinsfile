#!/usr/bin/env groovy
// vim: set ft=groovy:

def image = "invisionag/testimage:build-${env.BUILD_NUMBER}"



podTemplate(label: 'kubernetes', containers: [
  containerTemplate(
    name: 'docker',
    image: 'docker/compose:1.10.0',
    ttyEnabled: true,
    command: 'cat',
    envVars: [
      containerEnvVar(key: 'FORECAST_USER', value: '$password')
    ]
  )
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  stage ('Wrap Secrets') {
    node ('kubernetes') {
      container('docker') {
        def fc_secrets = [
            [$class: 'VaultSecret', path: 'secret/forecast/auth',
              secretValues: [
                [$class: 'VaultSecretValue', envVar: 'FORECAST_USER', vaultKey: 'user'],
                [$class: 'VaultSecretValue', envVar: 'FORECAST_PASSWORD', vaultKey:  'password']]]
        ]
        wrap([$class: 'VaultBuildWrapper', vaultSecrets: fc_secrets]) {
          sh 'echo $FORECAST_USER'
          sh 'echo $FORECAST_PASSWORD'
        }
      }
    }
  }
  
  stage ('Build') {
    node ('kubernetes') {
      container('docker') {
        try {
          wrap([$class: 'VaultBuildWrapper', vaultSecrets: fc_secrets]) {
            sh 'echo $FORECAST_USER'
            sh 'echo $FORECAST_PASSWORD'
          }
          sh 'docker-compose build --pull'
          git 'https://github.com/digitalocean/netbox.git'
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
