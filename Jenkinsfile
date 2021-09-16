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
                    api_image = docker.build('ubuntu-development:1.0', '--build-arg git_personal_token=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 -f Dockerfile.development .')
                    sh 'pwd'

                    docker.image('ubuntu-development:1.0').withRun("--name ubuntu_dev_bash -p 6200:6200 -p 27017:27017 ubuntu-development:1.0 /bin/bash"){
                        sh label:
                        'Running pwd',
                        script: '''
                        pwd
                        '''
                    }
                // docker.image('ubuntu-development:1.0').inside {
                //     sh 'pwd'
                //     sh 'ls -l'
                // }
                }
                echo 'Backend Portal Server is Deployed and Ready to use'
            }
        }
    }
}
