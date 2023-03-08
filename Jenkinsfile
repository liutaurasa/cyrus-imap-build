pipeline {
    agent any
    parameters {
        choice(name: 'TAG_TO_BUILD', choices: ['cyrus-imapd-3.6.1', 'main'], description: 'Specific tag to build')
    }
    stages {
        stage('Build') {
            steps {
                echo "Checkout Cyrus IMAP source ... ${params.TAG_TO_BUILD}"
                dir('cyrus-imapd') {
                    checkout([$class: 'GitSCM',
                              branches: [[name: "${params.TAG_TO_BUILD}"]],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [],
                              gitTool: 'Default',
                              submoduleCfg: [],
                              userRemoteConfigs: [[url: 'https://github.com/cyrusimap/cyrus-imapd.git']]
                            ])
                    sh 'ls -ltr'
                    sh 'autoreconf -i'
                    sh './configure'
                    sh 'make' 
                }
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
