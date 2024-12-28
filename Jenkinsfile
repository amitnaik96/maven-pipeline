pipeline {
    agent any
    tools {
        maven 'sonarmaven' // Replace with the name you assigned in Jenkins
    }
    environment {
        SONAR_SCANNER_HOME = 'C:/Program Files/sonar-scanner-6.2.1.4610-windows-x64/bin' // Update if required
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Use Maven tool configured in Jenkins
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Access SonarQube token from Jenkins credentials
            }
            steps {
             bat '''
                mvn clean verify sonar:sonar \
                  -Dsonar.projectKey=maven-pipeline \
                  -Dsonar.projectName="maven-pipeline" \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.token=$SONAR_TOKEN
                '''
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

