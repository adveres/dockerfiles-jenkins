# This Dockerfile is used to build an image containing basic stuff to be used as a Jenkins slave build node.
# It is based on instructions from:
#   * https://wiki.jenkins-ci.org/display/JENKINS/Docker+Plugin and Dockerfile
#   * https://hub.docker.com/r/evarga/jenkins-slave/
#   * https://hub.docker.com/r/stefanlehmann/centos-jenkins-docker-slave/
FROM centos:latest

# Install a basic SSH server GIT, UNZIP, LSOF, JDK8-devel
RUN yum -y update \
    && yum -y install openssh-server git svn unzip zip lsof java-1.8.0-openjdk-devel.x86_64 \
    && yum -y clean all

# update sshd settings, create jenkins user, set jenkins user pw, generate ssh keys
RUN sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd \
    && mkdir -p /var/run/sshd \
    && useradd -u 1000 -m -s /bin/bash jenkins \
    && echo "jenkins:jenkins" | chpasswd \
    && /usr/bin/ssh-keygen -A

# Make sure there's a maven folder
RUN mkdir /home/jenkins/.m2 \
    && chown jenkins:jenkins /home/jenkins/.m2

# Set JAVA_HOME
ENV JAVA_HOME /usr/lib/jvm/java

# Standard SSH port
EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
