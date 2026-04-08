pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh """
                    ${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=static-site \
                    -Dsonar.projectName=Static-Site \
                    -Dsonar.sources=src
                    """
                }
            }
        }
    }
}
