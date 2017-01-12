#!/usr/bin/env groovy
// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    [name: 'docker', image: 'docker/compose:1.9.0', ttyEnabled: true, command: 'cat']
],
volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {


stage 'Build'
node ('kubernetes') {
    container('docker') {
      try {
          git 'https://github.com/digitalocean/netbox.git'
          sh 'cd $WORKSPACE/netbox'
          sh 'docker-compose build --pull'
      } catch(err) {
          sh 'docker-compose down -v --remove-orphans'
        throw err
      } finally {
          sh 'docker-compose down -v --remove-orphans'
   }
  } // container
 } // node

stage 'Test'
node ('kubernetes') {
    container('docker') {
      try {
          sh 'docker-compose run --rm netbox manage.py test netbox/'
      } catch(err) {
          sh 'docker-compose down -v --remove-orphans'
        throw err
      } finally {
          sh 'docker-compose down -v --remove-orphans'
   }
  } // container
 } // node

} // end
