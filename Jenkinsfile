pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Checkout Cyrus IMAP source ...'
                checkout scmGit(
                branches: [[name: 'master']],
                userRemoteConfigs: [[url: 'https://github.com/cyrusimap/cyrus-imapd.git']])
                sh './configure'
                sh 'make' 
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
