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
                script {
                    docker.build('ubuntu-development:1.0', '--build-arg git_personal_token=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 -f Dockerfile.development .')
                    sh 'pwd'
                    sh 'docker run --name ubuntu_dev_bash -dit -p 6200:6200 -p 27017:27017 ubuntu-development:1.0 /bin/bash'
                    sh  label:
                    'Update Repo and start Mongod',
                    script:'''
                    echo "Update Repo and start Mongod"
                    set -x
                    docker exec -w /Development/ ubuntu_dev_bash pwd
                    docker exec -w /Development/ ubuntu_dev_bash ls -l
                    docker exec -w /Development/ ubuntu_dev_bash git pull
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash sudo mongod --port 27017 --dbpath /srv/mongodb/db0 --replSet rs0 --bind_ip localhost --fork --logpath /var/log/mongod.log
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash ps -ef
                    '''
                }

                script {
                    echo 'Build Binaries'
                    sh label:
                    'Build Binaries',
                    script:'''
                    set -x
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash sh CreateDailyBuild.sh
                    docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "ls -l"
                    docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseGateway  > database.log &"
                    sleep 1
                    docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./RestApiPortal > portal.log &"
                    sleep 1
                    docker exec -w /Development/Milestone3/ ubuntu_dev_bash ps -ef
                    # docker stop ubuntu_dev_bash
                    # docker rm ubuntu_dev_bash
                    # docker kill $(docker ps -q)
                    # docker rm $(docker ps -a -q)
                    '''
                }
                script {
                    try {
                        echo 'Load Database'
                        sh 'docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "ls -l"'
                        sh 'docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200"'
                    }catch (exception) {
                        echo getStackTrace(exception)
                        echo 'Error detected, retrying...'
                        sh '''
                            docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200 -d"
                            docker exec -w /Development/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200"
                            ''' 
                    }
                }
                echo 'Backend Portal Server is Deployed and Ready to use'

            }
        }
        stage('Run SailApiTAP') {
            steps {
                echo 'Starting to build docker image for test: SAILTAP'
                sh 'ls -l'
                script {
                    echo 'Update Test Repo'
                    docker.build('ubuntu-sailtap:1.0', '--build-arg git_personal_token=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 -f Dockerfile.test .')
                    sh 'docker run --name ubuntu_tst_bash -dit ubuntu-sailtap:1.0 /bin/bash'
                    sh  label:
                    'Update Test Repo',
                    script:'''
                    docker exec -w /Test/ ubuntu_tst_bash pwd
                    docker exec -w /Test/ ubuntu_tst_bash ls -l
                    docker exec -w /Test/ ubuntu_tst_bash git pull
                    '''
                    echo 'Running Tests'
                    sh  label:
                    'Running Tests',
                    script:'''
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/sail_portal_api_test.py --ip 10.0.0.5 -m active -sv --junitxml=sail-result.xml
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/account_mgmt_api_test.py --ip 10.0.0.5 -m active -sv --junitxml=account-mgmt-result.xml
                    docker cp ubuntu_tst_bash:/Test/sail-result.xml .
                    docker cp ubuntu_tst_bash:/Test/account-mgmt-result.xml .
                    docker cp ubuntu_dev_bash:/Development/Milestone3/Binary/portal.log .
                    docker cp ubuntu_dev_bash:/Development/Milestone3/Binary/database.log .
                    '''
                }
            }
            post {
                always {
                    // Post xml results of pytest run to Jenkins
                    echo 'End of stage test in Run SAILTAP!'
                    junit '*.xml'
                    archiveArtifacts artifacts: '*.log'
                }
            }
        }
    }
}
