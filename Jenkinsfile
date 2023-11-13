pipeline {
    agent {
        docker {
            image 'node:lts-buster-slim'
            args '-u root -p 3000:3000''
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Install Docker Compose') {
            steps {
                sh 'apt-get update && apt-get install -y docker-compose'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('View Logs') {
            steps {
                script {
                    sh 'apk add --no-cache docker-compose'  // Install Docker Compose
                    sh 'docker-compose logs > logs.txt'  // Save logs to a file
                }
            }
        }
    }
}
