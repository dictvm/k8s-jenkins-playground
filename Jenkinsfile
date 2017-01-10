// vim: set ft=groovy:

podTemplate(label: 'kubernetes', containers: [
    [name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62-alpine', args: '${computer.jnlpmac} ${computer.name}'],
    [name: 'python', image: 'python:3-alpine', ttyEnabled: true]
  ]) {
    node ('kubernetes') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('python') {
            stage 'Install requirements'
            sh 'pip3 -r requirements.txt'
    }
  }
}
