/* groovylint-disable DuplicateStringLiteral, LineLength, NestedBlockDepth */
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
                sh '''
                    git --version
                    pwd
                    ls -l /
                '''
                echo 'End of stage daily build!'
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
                sh '''
                    git --version
                    pwd
                    ls -l /
                '''
                echo 'End of stage Test!'
            }
        }
    }
}

// pipeline {
//     agent none {
//         stages {
//             stage('build') {
//                 stage('build-nightly') {
//                     agent {
//                         dockerfile {
//                             filename 'Dockerfile.dailybuilds'
//                             label 'docker'
//                             additionalBuildArgs '
//                             --build-arg git_personal_token=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
//                             --build-arg -t ubuntu-daily
//                             '
//                             customWorkspace './daily'
//                         }
//                     }
//                     steps {
//                         'Hello World from Daily Build'
//                         sh '''
//                             git --version
//                         '''
//                     }
//                 }

//                 stage('build-test') {
//                     agent {
//                         dockerfile {
//                             filename 'Dockerfile.test'
//                             label 'docker'
//                             additionalBuildArgs '
//                             --build-arg git_personal_token=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
//                             --build-arg -t ubuntu-test
//                             '
//                             customWorkspace './test'
//                         }
//                     }
//                     steps {
//                         'Hello World from test Build'
//                         sh '''
//                             git --version
//                         '''
//                     }
//                 }
//             }
//         }
            // stage('tests') {
            //     parallel {
            //         stage('test-php5.4') {
            //             agent {
            //                 dockerfile {
            //                     dir '/var/lib/jenkins/Docker'
            //                     filename 'Dockerfile-php5.4'
            //                     customWorkspace './build-php5.4'
            //                 }
            //             }
            //             steps {
            //                 sh 'php --version'
            //                 sh 'php vendor/phpunit/phpunit/phpunit tests'
            //             }
            //         }

            //         stage('test-php7.0') {
            //             agent {
            //                 dockerfile {
            //                     dir '/var/lib/jenkins/Docker'
            //                     filename 'Dockerfile-php7.0'
            //                     customWorkspace './build-php7.0'
            //                 }
            //             }
            //             steps {
            //                 sh 'php --version'
            //                 sh 'php vendor/phpunit/phpunit/phpunit tests'
            //             }
            //         }
            //     }
            // }
//     }
// }
