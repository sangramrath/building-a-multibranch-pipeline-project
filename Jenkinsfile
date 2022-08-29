pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
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
        stage('Deliver to Staging') {
            when {
                branch 'stage'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Done with development?'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy to Production') {
            when {
                branch 'live'
            }
            steps {
                sh './jenkins/scripts/deliver-for-production.sh'
                input message: 'Done with Deployment to Production?'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
