pipeline {
    agent { docker { image 'node:16.17.1-alpine' } }
    triggers {
        cron('H H(16-17) * * *')
    }
    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '50'))
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/Frederik88/Catvision-SPA.git'
            }
        }
        stage('Install') {
            steps {
                sh 'node --version'
                sh 'npm install'
            }
        }
        stage('Lint') {
            steps {
                sh 'ng lint'
            }
        }
        stage('Unit Test') {
            options {
                timeout(time: 4, unit: 'HOURS')
            }
            steps {
                sh 'npm run test --watch=false'
            }
        }
        stage('Build') {
            steps {
                sh 'ng build --prod'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}