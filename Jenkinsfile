pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'sonar'
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'devops-static-site-91928'
        CLOUDFRONT_ID = 'E3OAU258OPITEX'
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

        stage('Deploy to S3') {
            steps {
                sh 'aws s3 sync src/ s3://$S3_BUCKET --delete'
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                sh '''
                aws cloudfront create-invalidation \
                  --distribution-id $CLOUDFRONT_ID \
                  --paths "/*"
                '''
            }
        }
    }
}
