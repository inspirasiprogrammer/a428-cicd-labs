pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
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
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                sh 'sleep 1m'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Lanjutkan ke tahap Deploy? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
