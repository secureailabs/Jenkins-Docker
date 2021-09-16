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
                // sh '''
                //     pwd
                //     ls -l
                //     docker kill $(docker ps -q)
                //     docker rm $(docker ps -a -q)
                //     '''
                echo 'Starting to build docker image: Backend Api Portal Server'
                script {
                    /* groovylint-disable-next-line UnnecessaryGString */
                    docker.build("ubuntu-development:1.0", "--build-arg git_personal_token=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 -f Dockerfile.development .")
                    docker.image("ubuntu-development:1.0").inside {
                        sh 'pwd'
                        sh 'ls -l'
                    }
                }
                echo 'Backend Portal Server is Deployed and Ready to use'
            }
        }
    }
}
