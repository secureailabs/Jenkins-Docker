# Jenkins-Docker
[![Linter](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml/badge.svg?branch=main)](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml)

This is to store Jenkins Pipeline code and associated Dockerfiles for Test CI-CD \
Repository used for testing out DockerFiles in Jenkins

Dockerfile.web instructions: \
docker build --build-arg git_personal_token=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 -f Dockerfile.web -t sail-web:dev . \
docker-compose -f docker-compose.web.yml up
