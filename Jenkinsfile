/* groovylint-disable NestedBlockDepth */
pipeline {
    agent any
    options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '30'))
    parallelsAlwaysFailFast()
    timestamps()
    }
    environment {
        api_image = docker.build('ubuntu-development:1.0', '--build-arg git_personal_token=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 -f Dockerfile.development .')
    }
    stages {
        stage('Build Binaries & Deploy API Portal') {
            steps {
                // sh '''
                //     pwd
                //     ls -l
                //     docker kill $(docker ps -q)
                //     docker rm $(docker ps -a -q)
                //     '''
                echo 'Starting to build docker image: Backend Api Portal Server'
                script {
                    docker.build('ubuntu-development:1.0', '--build-arg git_personal_token=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 -f Dockerfile.development .')
                    sh 'pwd'
                    sh 'docker run --name ubuntu_dev_bash -dit -p 6200:6200 -p 27017:27017 ubuntu-development:1.0 /bin/bash'
                    sh label:
                    ' Update Repo & Start Mongodb'
                    script:'''
                    docker exec -w /Development/ ubuntu_dev_bash pwd
                    docker exec -w /Development/ ubuntu_dev_bash ls -l
                    docker exec -w /Development/ ubuntu_dev_bash git pull
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash sudo mongod --port 27017 --dbpath /srv/mongodb/db0 --replSet rs0 --bind_ip localhost --fork --logpath /var/log/mongod.log
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash ps -ef
                    '''
                }

                script {
                    sh label:
                    'Build Binaries',
                    script:'''
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash sh CreateDailyBuild.sh
                    '''
                    # docker exec -w /Development/Milestone3/ ubuntu_dev_bash sh CreateDailyBuild.sh
                    # docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseGateway  > database.log &"
                    # sleep 1
                    # docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./RestApiPortal > portal.log &"
                    # sleep 1
                    # docker exec -w /Development/Milestone3/ ubuntu_dev_bash ps -ef
                    # docker stop ubuntu_dev_bash
                    # docker rm ubuntu_dev_bash
                    # docker kill $(docker ps -q)
                    # docker rm $(docker ps -a -q)
                }
                echo 'Backend Portal Server is Deployed and Ready to use'
            }
        }
    }
}
