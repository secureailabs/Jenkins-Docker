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
            parallel {
                stage('Builds-Daily') {
                    agent {
                        dockerfile {
                            filename 'Dockerfile.dailybuilds'
                            label 'docker'
                            additionalBuildArgs '--build-arg git_personal_token=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'
                            customWorkspace './daily-build1'
                        }
                    }
                    steps {
                        echo 'Hello World from daily build!'
                        sh '''git --version
                                pwd
                                ls -l /
                                '''
                        echo 'End of stage daily build!'
                    }
                }
                stage('Builds-Development') {
                    agent {
                        dockerfile {
                            filename 'Dockerfile.development'
                            label 'docker'
                            additionalBuildArgs '--build-arg git_personal_token=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'
                            customWorkspace './development1'
                        }
                    }
                    steps {
                        echo 'Hello World from development!'
                        sh '''git --version
                                pwd
                                ls -l /
                                '''
                        echo 'End of stage development build!'
                    }
                }
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
                        stage('update-repository') {
                            // Run git pull to grab latest changes for docker container
                            steps {
                                echo 'Hello World!'
                                sh '''git --version
                                        ls -l
                                        pwd
                                        ls -l /
                                        cd /Test
                                        ls -l
                                        cd /Test
                                        git pull
                                        ls -l
                                    '''
                                echo 'End of stage update repository in Builds-Test!'
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
                                pytest /Test/StanleyLin/test_api/sail_api_test.py -m active -sv --junitxml=sail-result.xml
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
            }
        }
    }
}
