[![Linter](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml/badge.svg?branch=main)](https://github.com/secureailabs/Jenkins-Docker/actions/workflows/Linter.yml)

# Prerequisite
> **Note:** You cannot use `localhost` or `127.0.0.1` instead must use the actual public ip \
> If you are hosting backend and frontend on same host `backend_public_ip` and `frontend_public_ip` should match

- [Install docker engine](https://docs.docker.com/engine/install/) and ensure permissions
- [Install docker compose](https://docs.docker.com/compose/install/) and ensure permissions
- Make edit to .client.env:
```
SNOWPACK_PUBLIC_API_URL_PROD=https://<frontend_public_ip>:3000
```
- Make edit to .server.env:
```
SAIL_API=https://<backend_public_ip>:6200
CLIENT=https://<frontend_public_ip>:3000 
NODE_TLS_REJECT_UNAUTHORIZED="0" 
NODE_ENV=production
 ```
 - Generate new certificates in web-ui directory: \
Find absolute path of your web-ui directory; `pwd` on linux to replace `<abs_path_to_web-ui>` below...

    ```
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout <abs_path_to_web-ui>/ssl.key -out \ 
    <abs_path_to_web-ui>/ssl.crt
    ```

# WEB CONTAINER
### DEPLOY WEB CONTAINER
1. Docker Build: \
`docker build --build-arg git_personal_token=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 -f Dockerfile.web -t sail-web:local .`
2. Docker compose: \
`docker-compose -f docker-compose.web.yml up`

### STOP WEB CONTAINER
1. Docker compose down:
`docker-compose -f docker-compose.web.yml down`
