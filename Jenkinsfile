/* groovylint-disable DuplicateStringLiteral, LineLength, NestedBlockDepth */
pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.test'
            label 'docker'
            additionalBuildArgs '--build-arg git_personal_token=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'
            args '-t ubuntu_dailybuilds'
        }
    }
    stages {
        stage('Test') {
            steps {
                echo 'Hello World!'
                sh '''
                    git --version
                '''
                echo 'End of stage Test!'
            }
        }
    }
}
// pipeline {
//     agent any
//     stages {
//         stage('Build images') {
//             echo 'Starting to build docker images'
//             script {
//                 def custom_1 = dockerfile {
//                     filename 'Dockerfile.dailybuilds'
//                     label 'docker'
//                     additionalBuildArgs '--build-arg git_personal_token=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'
//                     args '-t ubuntu_dailybuilds'
//                 }
//                 custom_1.inside {
//                     echo 'Hello World from Daily Build'
//                     sh '''
//                         git --version
//                     '''
//                 }
//                 def custom_2 = dockerfile {
//                 filename 'Dockerfile.test'
//                 label 'docker'
//                 additionalBuildArgs '--build-arg git_personal_token=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'
//                 args '-t ubuntu_test'
//                 }
//                 custom_2.inside {
//                     echo 'Hello World from Test(Development)'
//                     sh '''
//                         git --version
//                     '''
//                 }
//             }
//         }
//     }
// }
