#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    [name: 'docker', image: 'docker/compose:1.9.0', ttyEnabled: true, command: 'cat']
]) {

  node ('kubernetes') {
    stage 'Check out demo repo'
      git 'https://github.com/digitalocean/netbox.git'
      container('docker') {
        stage 'Install requirements'
            sh 'docker-compose up'
      }
  }
}
