FROM redhat/ubi8-init

LABEL maintainer="Liutauras Adomaitis <liutauras.adomaitis at gmail.com>"

# Make sure the package repository is up to date.
RUN yum update -y && \
# Install a basic SSH server
    yum install -y openssh-server &&
# Install Development tools
    yum group install "Development Tools" && \
# Cleanup
    yum clean all && \
# Add user jenkins to the image
    useradd jenkins && \
# Set password for the jenkins user (you may want to alter this).
    echo "jenkins:jenkins" | passwd --stdin jenkins

# Copy authorized keys
COPY .ssh/authorized_keys /home/jenkins/.ssh/authorized_keys

RUN chown -R jenkins:jenkins /home/jenkins/.m2/ && \
    chown -R jenkins:jenkins /home/jenkins/.ssh/

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
