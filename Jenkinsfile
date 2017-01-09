// vim: set ft=groovy:

podTemplate(label: 'demopod', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62-alpine', args: '${computer.jnlpmac} ${computer.name}'),
//    containerTemplate(name: 'python', image: 'python:3-alpine', ttyEnabled: true)
  ]) {
    node ('demopod') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('python') {
            stage 'Install requirements'
            sh 'pip3 install -r requirements.txt'
    }
  }
}
