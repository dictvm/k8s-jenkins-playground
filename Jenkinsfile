// vim: set ft=groovy:

podTemplate(label: 'k8s', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:latest', ttyEnabled: true,
    command: 'cat')
  ]) {
    node ('k8s') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/k8s-jenkins-playground'
        container('alpine') {
            stage 'Install requirements'
            sh 'echo foo && sleep 600s'
    }
  }
}
