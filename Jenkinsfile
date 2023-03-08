pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Checkout Cyrus IMAP source ...'
                checkout scmGit(
                    branches: [[name: 'cyrus-imapd-3.6.1']],
                    userRemoteConfigs: [[url: 'https://github.com/cyrusimap/cyrus-imapd.git']]
                )
                sh 'autoreconf -i'
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
