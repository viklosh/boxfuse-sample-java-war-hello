pipeline {
    agent none

    stages {
        #-----------------------------------------
        stage('buid_docker_builder_image') {
            agent {
                docker {
                    image 'viklosh/docker_boxfuse_builder'
                    args  '-v artifact:/artifact'
                    args  '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            
            steps {
                echo 'Build Docker builder Image'
            }
        }
    }
}
