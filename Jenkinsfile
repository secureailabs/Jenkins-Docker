/* groovylint-disable NestedBlockDepth */
pipeline {
    agent none
    stages {
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
                stage('build') {
                    steps {
                        echo 'Hello World!'
                        sh '''git --version
                                ls -l
                                pwd
                                ls -l /

                            '''
                        echo 'End of stage Test!'
                    }
                }
                stage ('test') {
                    steps {
                        dir('/Test')
                        sh script: '''
                            #!/bin/bash
                            echo "This is current directory $(pwd)"
                            ls -l
                            // cd /Test
                            echo "This is your new directory $(pwd)"
                            ls -l
                        '''
                    }
                }
            }
        }

        // stage('Test-test') {
        //     agent {
        //         dockerfile {
        //             filename 'Dockerfile.test'
        //             label 'docker'
        //             additionalBuildArgs '--build-arg git_personal_token=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'
        //             customWorkspace './test-build1'
        //         }
        //     }
        //     steps {
        //         sh script: '''
        //         #!/bin/bash
        //         echo "This is current directory $(pwd)"
        //         cd /root/Test/
        //         echo "This is your new directory $(pwd)"
        //         ls -l
        //         '''
        //         sh 'pytest /root/Test/StanleyLin/test_api/sail_api_test.py -m active -sv --junitxml=reports/result.xml'
        //     }
        //     post {
        //         always {
        //             junit 'test-results/results.xml'
        //         }
        //     }
        // }
    }
}
