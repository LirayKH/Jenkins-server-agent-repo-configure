pipeline {
    // agent any
    agent {
        docker {
            label 'agent-remote'
            image 'jenkins/ssh-agent:alpine'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
            reuseNode true
        }
    }
    stages {
        stage('Build') {

            steps {
                // sh 'lsb_release -d'
		//sh "chmod +x -R ${env.WORKSPACE}"
                sh 'cat /etc/alpine-release '
            }
        }
    }
}
