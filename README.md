### ds-ex11
1. First of all you need to setup your environment to build and push docker_boxfuse_builder image. \
Use your own repository, or just skip first two steps to use image from my repo.
```
docker login
export DOCKER_BOXFUSE_BUILDER_TAG=viklosh/docker_boxfuse_builder
```
2. Build and push docker_boxfuse_builder image.
```
docker docker build https://github.com/viklosh/ds-ex11.git -f Dockerfile.docker_boxfuse_builder -t docker_boxfuse_builder
docker tag docker_boxfuse_builder ${DOCKER_BOXFUSE_BUILDER_TAG}
docker push ${DOCKER_BOXFUSE_BUILDER_TAG}
```
3. Setup Jenkins pipline
- install docker plagin
- create new pipeline
- set `GitHub hook trigger for GITScm polling` option.
- in `Pipeline` section set `Pipeline script from SCM.` `Repository URL - https://github.com/viklosh/ds-ex11.git`\
  `Просмотрщик репозитория - githubweb`, `URL - https://github.com/viklosh/ds-ex11`
- check `Pipeline Syntax` add `withCredentials: Bind credentials to variables.`, `Add > Username and password (separated)`, add dockerhub credentials.
4. In root of fork project create Dockerfile.boxfuse
```
FROM tomcat:10.0.2-jdk8-corretto
COPY target/hello-1.0.war /usr/local/tomcat/webapps
```
6. In root of fork project create Jenkinsfile.
```
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
        stage('deploy') {
            agent any
            steps {
                sshagent(['778a783f-aff1-46d0-aefe-5ca36c5971eb']) {
                   sh 'ssh -o StrictHostKeyChecking=no viklosh@10.130.0.25 docker run --rm --name boxfuse -d -p 8080:8888 viklosh/boxfuse'
                }
            }
        }
    }
}
```
