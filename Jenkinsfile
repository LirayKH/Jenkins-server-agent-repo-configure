pipeline {
    // agent any
    agent {
        docker {
            // image 'jenkins/ssh-agent:alpine'
            image 'ubuntu:latest'
            // args '-it --entrypoint=/bin/bash'
            label 'agent-remote'
                    // Run the container on the node specified at the
                    // top-level of the Pipeline, in the same workspace,
                    // rather than on a new node entirely:
            // reuseNode true
        }
    }
    stages {
        stage('Build') {
            steps {
                // sh 'lsb_release -d'
		//sh "chmod +x -R ${env.WORKSPACE}"
                sh 'echo $PWD'
            }
        }
    }
}
