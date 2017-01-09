// vim: set ft=groovy:

podTemplate(label: 'k8s', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:latest', ttyEnabled: true,
    command: 'cat')
  ]) {
    node ('k8s') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('alpine') {
            stage 'Install requirements'
            sh 'sleep 60s, echo "yay"'
    }
  }
}
