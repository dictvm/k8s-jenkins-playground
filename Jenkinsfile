#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    //    [name: 'jnlp', image: 'jenkinsci/jnlp-slave:latest', args: '${computer.jnlpmac} ${computer.name}'],
    [name: 'python', image: 'python:3-alpine', ttyEnabled: true, command:
    'cat'],
    [name: 'docker', image: 'docker/compose', ttyEnabled: true, command: 'cat']
  
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
