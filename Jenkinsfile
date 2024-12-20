pipeline {
    agent any
    environment {
        SSH_CREDENTIALS_ID = 'waleed' // Replace with your Jenkins SSH credentials ID
        EC2_HOST = '3.86.212.51' // Replace with your EC2 instance username and IP
        DEPLOY_DIR = '/var/www/html' // Apache's document root
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from Git repository...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Add build steps if required, e.g., npm, Maven, etc.
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying to Apache server on EC2 instance...'
                sshagent([SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $EC2_HOST 'mkdir -p $DEPLOY_DIR'
                        scp -o StrictHostKeyChecking=no -r * $EC2_HOST:$DEPLOY_DIR
                        ssh -o StrictHostKeyChecking=no $EC2_HOST 'sudo systemctl restart httpd'
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
