pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
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
