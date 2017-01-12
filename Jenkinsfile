#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    [name: 'docker', image: 'docker/compose:1.9.0', ttyEnabled: true, command: 'cat']
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {


stage 'Testing'
node ('kubernetes') {
  try {
    git 'https://github.com/digitalocean/netbox.git'
    container('docker')
      sh 'docker-compose build --pull'
      sh 'docker compose run --rm netbox manage.py test netbox/'
  } catch(err) {
      sh 'docker-compose down -v --remove-orphans'
    throw err
  } finally {
      sh 'docker-compose down -v --remove-orphans'
  }
 }
}
