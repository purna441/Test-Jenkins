pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/purna441/Test-Jenkins.git',
                    credentialsId: 'github-credentials'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('myapp') {    // 👈 Change this to the folder containing pom.xml
                    sh 'mvn clean package'
                }
            }
        }

        stage('SonarQube Analysis') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    dir('myapp') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying to Tomcat...'
            }
        }
    }

    post {
        failure {
            echo '❌ Build or deployment failed!'
        }
        success {
            echo '✅ Build successful!'
        }
    }
}
