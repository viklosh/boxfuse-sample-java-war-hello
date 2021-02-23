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
4. In root of fork project create Dockerfile.boxfuse
5. In root of fork project create Jenkinsfile.
