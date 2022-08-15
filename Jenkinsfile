pipeline {
    agent any
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'rpplabs/stack-builder-centos-7:0.4.0'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
                    reuseNode true
                }
            }
            steps {
                sh 'lsb_release -d'
            }
        }
    }
}