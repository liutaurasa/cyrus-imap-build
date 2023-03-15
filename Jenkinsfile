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
                //        jansson-devel texinfo \
                //        libicu-devel zlib-devel \
                //  dnf install CUnit-devel clamav-devel cyrus-sasl-md5 cyrus-sasl-plain glibc-langpack-en groff jansson-devel krb5-devel libical-devel libicu-devel libnghttp2-devel libpq-devel mariadb-connector-c-devel net-snmp-devel openldap-devel pcre-devel rsync shapelib-devel systemd transfig xapian-core-devel
                sh """
                    dnf install -qy \
                        @c-development check cmake git which \
                        sqlite-devel file-devel diffutils cyrus-sasl-devel \
                        openssl-devel glib2-devel texinfo \
                        uuid-devel libxml2-devel zlib-devel \
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
