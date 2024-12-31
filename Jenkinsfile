pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-token') 
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                    // ${MAVEN_HOME}/bin/mvn 
                withSonarQubeEnv('sonarqube-server') { 
                    bat 
                    '
                    mvn clean verify sonar:sonar ^
                    -Dsonar.projectKey=hello-world ^
                    -Dsonar.projectName=Hello World ^
                    -Dsonar.login=${SONARQUBE_TOKEN} ^
                    -Dsonar.sources=src/main/java/com/example ^
                    -Dsonar.host.url=http://localhost:9000 
                    '
                }
            }
                    
        }
    }

    post {
        success{
            echo 'Pipeline Successfull'
        }
        failure{
            echo 'Pipeline Failure'
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
