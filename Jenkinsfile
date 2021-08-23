pipeline {
    agent none
    stages {
        stage('daily-builds') {
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
        stage('development') {
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

        stage('test-builds') {
            agent {
                dockerfile {
                    filename 'Dockerfile.test'
                    label 'docker'
                    additionalBuildArgs '--build-arg git_personal_token=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'
                    customWorkspace './test-build1'
                }
            }
            steps {
                echo 'Hello World!'
                sh '''git --version
                        pwd
                        ls -l /
                    '''
                echo 'End of stage Test!'
            }
        }
    }
}
