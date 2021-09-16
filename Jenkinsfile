/* groovylint-disable NestedBlockDepth */
pipeline {
    agent any
    options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '30'))
    parallelsAlwaysFailFast()
    timestamps()
    }
    stages {
        stage('Build Binaries & Deploy API Portal') {
            steps {
                sh '''
                    pwd
                    ls -l
                    docker kill $(docker ps -q)
                    docker rm $(docker ps -a -q)
                    '''
                echo 'Starting to build docker image: Backend Api Portal Server'
                def ApiPortal = docker.build("ubuntu-development:1.0", "--build-arg git_personal_token=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 -f Dockerfile.development .")
                echo 'Run Backend docker image in background'
                ApiPortal.inside {
                    sh label:
                    'start mongodb',
                    script: '''
                    pwd
                    ls -l
                    git pull
                    sudo mongod --port 27017 --dbpath /srv/mongodb/db0 --replSet rs0 --bind_ip localhost --fork --logpath /var/log/mongod.log
                    ps -ef
                    '''
                }
                echo 'Backend Portal Server is Deployed and Ready to use'
            }
        }
    }
}
