# Jenkins-Docker
[![Linter](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml/badge.svg?branch=main)](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml)

This is to store Jenkins Pipeline code and associated Dockerfiles for Test CI-CD \
Repository used for testing out DockerFiles in Jenkins

### PREREQUISITE
1. Please follow the steps to [install docker engine](https://docs.docker.com/engine/install/ubuntu/ ) on your distro 
    There is no reason not to install the latest stable version of docker engine. 
2. Clone down this repository
3. [In Github setup your own personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

### Docker Time
1. Build your docker images
```
cd Jenkins-Docker/
docker build -t <Image_tag: ex: ubuntu-development:1.0> --build-arg git_personal_token=<github_personal_access_token> -f <location_for_dockerfile> .
docker build -t test_development:1.0 --build-arg git_personal_token=asjhdgasjhcda -f Nightly_Tests/Dockerfile.development .
```
2. Verify docker image has been built sucessfully with command:
```
docker images
```
3. Start your docker as background
```
docker run --name <container_name: ex: ubuntu_dev_bash> -dit -p <local_port_to_expose:container_port> <container_image_tag> /bin/bash
docker run --name ubuntu_dev_bash -dit -p 6200:6200 -p 27017:27017 ubuntu-development:1.0 /bin/bash
```
4. Verify running containers with command:
```
docker ps
```
5. Verify stopped containers with command:
```
docker ps -a
```
6. To bring docker container to foreground run command:
```
docker attach <docker_container_name>
docker attach ubuntu_dev_bash
```
7. To detach from your docker connection back to vm, run command:
```
Press Ctrl-P, followed by Ctrl-Q
```
8. Alternatively, to execute commands from vm to container:
```
docker exec -w </specify/alternate/workdir/in/docker/> <container_name> ls -l
docker exec -w /Development/ ubuntu_dev_bash ls -l
```
9. To stop/kill docker container
```
docker stop <containter-id/container_name>
docker stop ubuntu_dev_bash
docker kill <containter-id/container_name>
docker kill ubuntu_dev_bash
```
10. To remove/delete/de-provision docker container, run command:
```
docker rm  <containter-id/container_name>
docker rm ubuntu_dev_bash
```
