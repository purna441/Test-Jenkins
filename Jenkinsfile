pipeline {
    agent any

    environment {
        // Update these for your environment
        GIT_REPO = 'https://github.com/purna441/https://github.com/purna441/Test-Jenkins.git'
        SONARQUBE_SERVER = 'sonarqube'
        SONARQUBE_TOKEN = credentials('Test-Jenkins')
        NEXUS_URL = 'http://52.66.250.140/:8082/repository/maven-releases/'
        NEXUS_CREDENTIALS = credentials('nexus-credentials')
        MAVEN_HOME = tool name: 'Maven', type: 'maven'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build with Maven') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean install -DskipTests"
            }
        }

        
                }
            }
        }stage('SonarQube Analysis') {
    steps {
        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
            sh '''
            mvn sonar:sonar \
              -Dsonar.projectKey=Test-Jenkins \
              -Dsonar.host.url=https://sonarcloud.io:9000 \
              -Dsonar.login=$SONAR_TOKEN
            '''
        }
    }
}


        stage('Upload to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'http://52.66.250.140/:8082',
                    groupId: 'com.example',
                    version: '1.0.0',
                    repository: 'maven-releases',
                    credentialsId: 'nexus-credentials',
                    artifacts: [
                        [artifactId: 'yourapp', classifier: '', file: 'target/yourapp.war', type: 'war']
                    ]
                )
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                echo "Deploying WAR to Tomcat..."
                sudo cp target/yourapp.war /opt/tomcat9/webapps/
                sudo systemctl restart tomcat9 || sudo /opt/tomcat9/bin/shutdown.sh; sudo /opt/tomcat9/bin/startup.sh
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Build and deployment successful!"
        }
        failure {
            echo "❌ Build or deployment failed!"
        }
    }
}

