pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Checkout Cyrus IMAP source ...'
                git url: 'https://github.com/cyrusimap/cyrus-imapd.git', branche: 'cyrus-imapd-3.6.1'
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
