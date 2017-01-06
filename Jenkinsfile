# vim: :set syntax=groovy

podTemplate(label: 'demopod', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:latest', ttyEnabled: true)
  ]) {
    node ('demopod') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/k8s-jenkins-playground.git'
        container('alpine') {
            stage 'Build a demo project'
            sh 'echo "this should run inside of a pod"'
    }
  }
}
