pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Replace with the name you assigned in Jenkins
    }
    environment {
        SONAR_SCANNER_HOME = 'C:/Program Files/sonar-scanner-6.2.1.4610-windows-x64/bin' // Update if required
        SONAR_TOKEN = credentials('sonar-token') // Access SonarQube token from Jenkins credentials
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    bat '''
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=maven-pipeline \
                        -Dsonar.projectName="maven-pipeline" \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=$SONAR_TOKEN
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

