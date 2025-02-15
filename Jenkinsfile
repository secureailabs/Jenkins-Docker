/* groovylint-disable NestedBlockDepth */
pipeline {
    agent {label 'Nightly' }
    options {
    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
    parallelsAlwaysFailFast()
    timestamps()
    }

    stages {
        stage('Git') {
            steps {
                sh '''
                    pwd
                    ls -l
                    # docker kill $(docker ps -q)
                    # docker rm $(docker ps -a -q)
                    '''
                echo 'Starting to build docker image: Backend Api Portal Server'
                script {
                    docker.build('ubuntu-development:1.0', '--build-arg git_personal_token=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 -f Nightly_Tests/Dockerfile.development .')
                    sh 'pwd'
                    sh 'docker run --name ubuntu_dev_bash -dit -p 6200:6200 -p 27017:27017 ubuntu-development:1.0 /bin/bash'
                    sh  label:
                    'Update Repo and start Mongod',
                    script:'''
                    echo "Update Repo and start Mongod"
                    set -x
                    docker exec -w /Engineering/ ubuntu_dev_bash pwd
                    docker exec -w /Engineering/ ubuntu_dev_bash ls -l
                    docker exec -w /Engineering/ ubuntu_dev_bash git pull
                    docker exec -w /Engineering/Milestone3/ ubuntu_dev_bash sudo mongod --port 27017 --dbpath /srv/mongodb/db0 --replSet rs0 --bind_ip localhost --fork --logpath /var/log/mongod.log
                    docker exec -w /Engineering/Milestone3/ ubuntu_dev_bash ps -ef
                    '''
                }
            }
            post {
                failure {
                    echo "Failed during Git stage"
                }
            }

        }
        stage('Build Backend') {
            steps {
                script {
                    echo 'Build Binaries'
                    sh label:
                    'Build Binaries',
                    script:'''
                    set -x
                    docker exec -w /Engineering/Milestone3/ ubuntu_dev_bash ./CreateDailyBuild.sh
                    docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "ls -l"
                    '''
                }
            }
            post {
                failure {
                    echo "Failed during Build Backend stage"
                }
            }
        }
        stage ('Deploy Backend'){
            steps {
                script {
                    echo 'Deploy DatabaseGateway and RestApiPortal'
                    sh '''
                    docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseGateway  > database.log &"
                    sleep 1
                    docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./RestApiPortal > portal.log &"
                    sleep 1
                    docker exec -w /Engineering/Milestone3/ ubuntu_dev_bash ps -ef
                    '''
                }
                script {
                    try {
                        echo 'Load Database'
                        sh 'docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "ls -l"'
                        sh 'docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200"'
                    }catch (exception) {
                        echo getStackTrace(exception)
                        echo 'Error detected, retrying...'
                        sh '''
                            docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200 -d"
                            docker exec -w /Engineering/Milestone3/Binary ubuntu_dev_bash sh -c "sudo ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200"
                            ''' 
                    }
                }
                echo 'Backend Portal Server is Deployed and Ready to use'
            }
            post {
                failure {
                    echo "Failed during Deploy Backend stage"
                }
            }
        }
        stage('Run SailApiTAP') {
            steps {
                echo 'Starting to build docker image for test: SAILTAP'
                sh 'ls -l'
                script {
                    echo 'Update Test Repo'
                    docker.build('ubuntu-sailtap:1.0', '--build-arg git_personal_token=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 -f Nightly_Tests/Dockerfile.test .')
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
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/dataset_mgmt_api_test.py --ip 10.0.0.5 -m active -sv --junitxml=dataset_mgmt.xml
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/digital_contract_mgmt_test.py --ip 10.0.0.5 -m active -sv --junitxml=digital_contract_mgmt.xml
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/azure_template_mgmt_test.py --ip 10.0.0.5 -m active -sv --junitxml=azure_template_mgmt.xml
                    docker exec -w /Test/ ubuntu_tst_bash pytest /Test/StanleyLin/test_api/datasetfamily_mgmt_api_test.py --ip 10.0.0.5 -m "active or active_m3" -sv --junitxml=dataset_family.xml
                    docker cp ubuntu_tst_bash:/Test/sail-result.xml .
                    docker cp ubuntu_tst_bash:/Test/account-mgmt-result.xml .
                    docker cp ubuntu_tst_bash:/Test/dataset_mgmt.xml .
                    docker cp ubuntu_tst_bash:/Test/digital_contract_mgmt.xml .
                    docker cp ubuntu_tst_bash:/Test/azure_template_mgmt.xml .
                    docker cp ubuntu_tst_bash:/Test/dataset_family.xml .
                    docker cp ubuntu_dev_bash:/Engineering/Milestone3/Binary/portal.log .
                    docker cp ubuntu_dev_bash:/Engineering/Milestone3/Binary/database.log .
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
                failure {
                    echo "Failed during Run SailApiTAP stage"
                }
            }
        }
    }
    post {
        always {
            echo 'Teardown'
            sh label:
            'Teardown',
            script:'''
            set -x
            docker kill $(docker ps -q)
            docker rm $(docker ps -a -q)
            '''
            cleanWs()
        }
    }
}
