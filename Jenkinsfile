/* groovylint-disable NestedBlockDepth */
pipeline {
    agent none
    options {
    buildDiscarder(logRotator(numToKeepStr: '30', artifactNumToKeepStr: '30'))
    parallelsAlwaysFailFast()
    timestamps()
    }
    stages {
        stage ('Parallel Stage') {
            // Start Parallel stage
            // parallel {
                // stage('Builds-Daily') {
                //     agent {
                //         dockerfile {
                //             filename 'Dockerfile.dailybuilds'
                //             label 'docker'
                //             additionalBuildArgs '--build-arg git_personal_token=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'
                //             customWorkspace './daily-build1'
                //         }
                //     }
                //     steps {
                //         echo 'Hello World from daily build!'
                //         sh '''git --version
                //                 pwd
                //                 ls -l /
                //                 '''
                //         echo 'End of stage daily build!'
                //     }
                // }
            stage('Builds-Development') {
                agent {
                    dockerfile {
                        filename 'Dockerfile.development'
                        label 'docker'
                        additionalBuildArgs '--build-arg git_personal_token=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'
                        args '-p 6200:6200 -p 27017:27017'
                        customWorkspace './development1'
                    }
                }
                stages {
                    stage('Build') {
                        // Run git pull to grab latest changes for docker container
                        steps {
                            echo 'Hello World from development!'
                            sh '''git --version
                                    pwd
                                    ls -l /
                                    cd /Development
                                    git pull
                                    ls -l
                                    cd /Development/Milestone3
                                    ls -l
                                    ./CreateDailyBuild.sh
                                    cd Binary/
                                    sudo mongod --port 27017 --dbpath /srv/mongodb/db0 --replSet rs0 --bind_ip localhost --fork --logpath /var/log/mongod.log
                                    ./DatabaseGateway &
                                    ./RestApiPortal &
                                    ls -l
                                    '''
                            sh '''
                                cd /Development/Binary/
                                ./DatabaseTools --PortalIp=127.0.0.1 --Port=6200
                                '''
                            echo 'End of stage Build in Builds-Development!'
                        }
                    }
                }
            }
            // stage('Run Api Tests') {
            //     steps {
            //         sh '''
            //             cd /root/SAIL/ScratchPad/StanleyLin/

            //             '''
            //         sh '''
            //             cd /root/SAIL/ScratchPad
            //             git pull
            //             pytest /root/SAIL/ScratchPad/StanleyLin//test_api/sail_portal_api_test.py -m active -ip 10.0.0.5 -sv --junitxml=sail-result.xml
            //             ls -l
            //             pytest /root/SAIL/ScratchPad/StanleyLin/test_api/account_mgmt_api_test.py -m active -sv -ip 10.0.0.5 --junitxml=account-mgmt-result.xml
            //             '''
            //     }
            //     post {
            //         always {
            //             // Post xml results of pytest run to Jenkins
            //             echo 'End of stage test in Builds-Test!'
            //             junit '*.xml'
            //         }
            //     }
            // }
            stage('Builds-Test') {
                agent {
                    dockerfile {
                        filename 'Dockerfile.test'
                        label 'docker'
                        additionalBuildArgs '--build-arg git_personal_token=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'
                        customWorkspace './test-build1'
                    }
                }
                stages {
                    stage('Build') {
                        // Run git pull to grab latest changes for docker container
                        steps {
                            echo 'Hello World!'
                            sh '''git --version
                                    ls -l
                                    pwd
                                    ls -l /
                                    cd /Test
                                    ls -l
                                    git pull
                                    ls -l
                                '''
                            echo 'End of stage Build in Builds-Test!'
                        }
                    }
                    stage ('test') {
                        // Run Nightly API tests
                        steps {
                            sh '''
                                echo "This is current directory $(pwd)"
                                ls -l
                                echo "This is your new directory $(pwd)"
                                ls -l
                            '''
                            sh '''
                            pytest /Test/StanleyLin/test_api/sail_portal_api_test.py -m active -sv --junitxml=sail-result.xml
                            ls -l
                            pytest /Test/StanleyLin/test_api/account_mgmt_api_test.py -m active -sv --junitxml=account-mgmt-result.xml
                            '''
                        }
                        post {
                            always {
                                // Post xml results of pytest run to Jenkins
                                echo 'End of stage test in Builds-Test!'
                                junit '*.xml'
                            }
                        }
                    }
                }
            }
            // }
        }
    }
}
