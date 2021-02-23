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
                sh 'docker build . -f Dockerfile.boxfuse -t viklosh/boxfuse'
                withCredentials([usernamePassword(credentialsId: '30aeb200-a5a5-48e6-9269-9f369d85a4df', passwordVariable: 'dockerhub_pass', usernameVariable: 'dockerhub_user')]) {
                    sh 'docker login -u ${dockerhub_user} -p ${dockerhub_pass}'
                }
                sh 'docker push viklosh/boxfuse'
            }
        }
    }
}
