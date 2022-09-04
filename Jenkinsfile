pipeline {
    agent {
        docker {
            image 'ubuntu:latest'
            label '{your-custom-lable}'
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
