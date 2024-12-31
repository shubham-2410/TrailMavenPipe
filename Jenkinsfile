pipeline {
    agent any

    tools {
        maven 'sonarmaven' // Ensure this is the correct name of your Maven tool in Jenkins
    }

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token') // Use Jenkins credentials for SonarQube token
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'mvn clean compile'  // Compile the project
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('sonarqube-server') {  // Ensure this matches the SonarQube server name configured in Jenkins
                    bat '''
                    mvn clean verify sonar:sonar ^
                    -Dsonar.projectKey=TrailMavenPipe ^
                    -Dsonar.login=${SONARQUBE_TOKEN} ^
                    -Dsonar.sources=src/main/java/com/example ^
                    -Dsonar.host.url=http://localhost:9000
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline was successful!'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
