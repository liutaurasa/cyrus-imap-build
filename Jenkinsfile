pipeline {
    agent any
    parameters {
        gitParameter name: 'TAG',
                     type: 'PT_TAG',
                     defaultValue: 'main'
    }
    echo 'Checkout Cyrus IMAP source ...'
    checkout([$class: 'GitSCM',
              branches: [[name: "${params.TAG}"]],
              doGenerateSubmoduleConfigurations: false,
              extensions: [],
              gitTool: 'Default',
              submoduleCfg: [],
              userRemoteConfigs: [[url: 'https://github.com/cyrusimap/cyrus-imapd.git']]
            ])
    stages {
        stage('Build') {
            steps {
                sh 'ls -ltr'
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
