pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'                     // your region
        S3_BUCKET = 'your-s3-bucket-name'            // your S3 bucket
        BUILD_DIR = 'dist'                           // your frontend build directory
        NODE_OPTIONS = '--openssl-legacy-provider'   // fix for crypto errors in Node 17+
    }

    stages {
        stage('Checkout Code') {
            steps {
                // If using private repo, add credentialsId
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/your-username/your-repo.git',
                        credentialsId: 'your-credentials-id' // Set this in Jenkins Credentials
                    ]]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Upload to S3') {
            steps {
                sh '''
                    echo "Uploading $BUILD_DIR to S3..."
                    aws s3 cp $BUILD_DIR s3://$S3_BUCKET/ --recursive --region $AWS_REGION
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployed to S3 successfully!'
        }
        failure {
            echo '❌ Pipeline failed. See logs.'
        }
    }
}
