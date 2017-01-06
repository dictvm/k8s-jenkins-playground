# vim: :set syntax=groovy

podTemplate(label: 'k8s', containers: [
    containerTemplate(name: 'python', image: 'python3:latest', ttyEnabled: true)
  ]) {
    node ('demopod') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('python3') {
            stage 'Install requirements'
            sh 'pip3 install -r requirements.txt'
    }
  }
}
