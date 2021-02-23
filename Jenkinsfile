pipeline {
    agent none

    stages {
        stage('buid_docker_builder_image') {
            agent {
                docker {
                    image 'viklosh/docker_boxfuse_builder'
                    args  '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            
            steps {
                sh 'docker build -f Dockerfile.boxfuse -t viklosh/boxfuse'
                sh 'ls -la'
            }
        }
    }
}
