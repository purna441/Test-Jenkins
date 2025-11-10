pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/purna441/Test-Jenkins.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                    mvn sonar:sonar \
                        -Dsonar.projectKey=Test-Jenkins \
                        -Dsonar.host.url=http://<your-sonarqube-server>:9000 \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'echo "Deploying artifact to Tomcat..."'
                // Example if you have SSH setup:
                // sh 'scp target/*.war ubuntu@<tomcat-server-ip>:/opt/tomcat/webapps/'
            }
        }
    }

    post {
        success {
            echo '✅ Build, analysis, and deployment completed successfully!'
        }
        failure {
            echo '❌ Build or deployment failed!'
        }
    }
}
