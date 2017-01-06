// vim: :set syntax=groovy

podTemplate(label: 'demopod', containers: [
    containerTemplate(name: 'alpine', image: 'alpine:latest', ttyEnabled: true)
  ],
    node ('demopod') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/nexus_checker.git'
        container('alpine') {
            stage 'Install requirements'
            sh 'pip3 install -r requirements.txt'
    }
}
