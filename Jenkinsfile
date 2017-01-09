// vim: set ft=groovy:

podTemplate(label: 'jnlp-slave', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62-alpine', args: '${computer.jnlpmac} ${computer.name}')
//    containerTemplate(name: 'python', image: 'python:3-alpine', ttyEnabled: true)
  ]) {
    node ('jnlp-slave') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('python') {
            stage 'Install requirements'
            sh 'sleep 900s'
    }
  }
}
