pipeline {
    agent {
        dockerfile {
            filename 'Dockerfile.test'
            label 'docker'
            additionalBuildArgs '--build-arg git_personal_token=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'
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
