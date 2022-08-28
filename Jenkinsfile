pipeline {
    agent {
        docker {
            // image 'ubuntu:latest'
            image 'ghcr.io/rpplabs/stack-builder-centos-7:0.4.0'
            // args '-it --entrypoint=/bin/bash'
            label 'rpplabs-remote-agent'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'echo $PWD'
            }
        }
    }
}
