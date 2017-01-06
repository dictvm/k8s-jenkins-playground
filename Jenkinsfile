podTemplate(label: 'demopod', containers: [
    containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:2.62-alpine', args: '${computer.jnlpmac} ${computer.name}'),
    containerTemplate(name: 'alpine', image: 'alpine3:latest', ttyEnabled: true,
    command: 'echo "foo"')
  ],
    node ('demopod') {
        stage 'Check out demo repo'
        git 'https://github.com/dictvm/k8s-jenkins-playground.git'
        container('alpine') {
            stage 'Build a demo project'
            sh 'echo "this should run inside of a pod"'
    }
}
