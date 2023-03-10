pipeline {
    agent {
        kubernetes {
            yamlFile 'Jenkins-pod.yaml'
            defaultContainer 'fedora'
        }
    }
    parameters {
        choice(name: 'TAG_TO_BUILD', choices: ['cyrus-imapd-3.6.1', 'main'], description: 'Specific tag to build')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Prepare container ... '
                sh """
                    dnf install -qy \
                        @c-development check cmake git \
                        sqlite-devel file-devel \
                        openssl-devel glib2-devel \
                        jansson-devel texinfo \
                        uuid-devel libxml2-devel \
                        libicu-devel zlib-devel \
                        graphviz-devel doxygen python3-docutils help2man && \
                    dnf clean all
                """
                echo "Checkout and Build Cyrus Libraries ..."
                dir('cyruslibs') {
                    git 'https://github.com/cyrusimap/cyruslibs'
                    sh 'ls -ltr'
                    sh './build.sh'
                    sh 'ls -ltr /usr/loca/cyruslibs'
                }
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
