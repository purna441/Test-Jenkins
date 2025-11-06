pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/purna441/Test-Jenkins.git',
                    credentialsId: 'github-credentials'
            }
        }
    }
}
