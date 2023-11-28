pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/m0719/appjen.git'
            }
        }
        
        stage('Build and Test') {
            steps {
                script {
                    // Assuming docker-compose.yml is in the root directory of the repository
                    bat 'docker-compose up -d'  // Use -d for detached mode, if needed
                }
            }
        }
        
        stage('Send Slack Notifications') {
            steps {
                script {
                    slackSend(
                        botUser: true,
                        channel: '#general',
                        message: 'Jobs committed',
                        tokenCredentialId: 'slacknewtoken'
                    )
                }
            }
        }
    }
}

