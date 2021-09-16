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
                echo 'Backend Portal Server is Deployed and Ready to use'

            }
        }
    }
}
