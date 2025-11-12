pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/purna441/Test-Jenkins.git'
            }
        }

        stage('Build with Maven') {
            steps {
                dir('myapp') {   // 👈 go into the folder containing pom.xml
                    sh 'mvn clean package'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('myapp') {
                    withSonarQubeEnv('sonarqube') {
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
    }
}
