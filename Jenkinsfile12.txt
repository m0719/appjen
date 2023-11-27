pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/m0719/appjen.git'
            }
        }
        
        stage('Check Installations') {
            steps {
                script {
                    bat 'docker --version'
                    bat 'python --version'
                    bat 'docker-compose --version'
                }
            }
        }
        
        stage('Remove Old Containers') {
            steps {
                script {
                    bat 'docker container prune -f' // Remove stopped containers forcefully
                }
            }
        }
        
        stage('Remove Old Images') {
            steps {
                script {
                    bat 'docker image prune -a -f' // Remove unused images forcefully
                }
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
    }
}
