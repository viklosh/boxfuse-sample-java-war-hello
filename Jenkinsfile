pipeline {
    agent none

    stages {
        stage('buid_docker_builder_image') {
            agent {
                dockerfile {
                    filename 'Dockerfile'
                    args '-v /tmp:/tmp'
                }
            }
            
            steps {
                echo 'Build Docker builder Image'
            }
        }
        stage('buid') {
            agent {
                docker { image 'maven:3-alpine' }
            }
            steps {
                sh 'mvn --version'
            }
        }
    }
}
