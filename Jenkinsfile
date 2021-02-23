pipeline {
    agent none

    stages {
        stage('buid_docker_builder_image') {
            agent {
                docker {
                    image 'viklosh/docker_boxfuse_builder'
                    args  '-v /var/run/docker.sock:/var/run/docker.sock -u 0:0'
                }
            }
            
            steps {
                sh 'mvn package'
                //sh 'docker build . -f Dockerfile.boxfuse -t viklosh/boxfuse'
                sh 'ls -la'
                sh 'ls target -la'
            }
        }
    }
}
