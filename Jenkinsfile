pipeline {
    agent any

    environment {
        AWS_REGION = 'us-east-1'
        S3_BUCKET = 'your-s3-bucket-name'
        BUILD_DIR = 'dist'
        NODE_OPTIONS = '--openssl-legacy-provider'
    }

    stages {
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
