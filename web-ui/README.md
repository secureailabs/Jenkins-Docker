[![Linter](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml/badge.svg?branch=main)](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml)

# WEB CONTAINER
    Build:
        1. docker build --build-arg git_personal_token=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 -f Dockerfile.web -t sail-web:local .
        2. docker-compose -f docker-compose.web.yml up
