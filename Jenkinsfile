pipeline {
    agent {
        kubernetes {
            yamlFile 'Jenkins-pod.yaml'
            defaultContainer 'fedora'
        }
    }
    parameters {
        choice(name: 'VERSION', choices: ['3.6.1', 'main'], description: 'Get specific version SRC RPM')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Prepare container ... '
                sh """
                    dnf install -qy wget rpm-build \
                        autoconf automake bison flex gcc-c++ git glib2-devel \
                        libtool libxml2-devel perl-Pod-Html perl-generators sqlite-devel \
                        CUnit-devel clamav-devel cyrus-sasl-md5 cyrus-sasl-plain \
                        glibc-langpack-en groff jansson-devel krb5-devel libical-devel \
                        libicu-devel libnghttp2-devel libpq-devel mariadb-connector-c-devel \
                        net-snmp-devel openldap-devel pcre-devel rsync shapelib-devel \
                        systemd transfig xapian-core-devel && \
                    dnf clean all
                """
                echo "Get cyrus-imapd src rpm package ..."
                sh """
                    wget https://mirror.apheleia-it.ch/repos/Kolab:/16:/Testing/CentOS_8_Stream/src/cyrus-imapd-${params.VERSION}-4.1.el8.kolab_16.src.rpm && \
                    rpm -ihv cyrus-imapd-${params.VERSION}-4.1.el8.kolab_16.src.rpm
                """
                echo "Building RPM packages v${params.VERSION}"
                sh "rpm-build -bb /root/rpmbuild/SPECS/cyrus-imapd.spec"
                sh "ls -lart"
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
