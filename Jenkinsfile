// vim: set ft=groovy:

podTemplate(label: 'k8s', containers: [
    containerTemplate(name: 'python', image: 'python3:latest', ttyEnabled: true)
  ]) {
    node ('python') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('python') {
            stage 'Install requirements'
            sh 'pip3 install -r requirements.txt'
    }
  }
}
