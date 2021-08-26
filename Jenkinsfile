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
            parallel {
                stage('builds-daily') {
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
                stage('builds-development') {
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
                stage('builds-test') {
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
                                echo 'End of stage Test!'
                            }
                        }
                        stage ('test') {
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
