FROM jenkins/jenkins:2.176.3
LABEL maintainer="whocares@gmail.com"

# Assume root privs
USER root
RUN apt-get update && apt-get install -y vim socat cron sudo apt-transport-https ca-certificates curl software-properties-common
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
RUN apt-get update
RUN apt-cache policy docker-ce
RUN apt-get install -y docker-ce
RUN sed -i 's/^.sudo.*/%sudo ALL=(ALL:ALL) NOPASSWD:ALL/' /etc/sudoers
RUN adduser jenkins sudo
RUN adduser jenkins docker

# Folders required for backup plugin
RUN mkdir /var/jenkins_home_backup \
 && chown jenkins: /var/jenkins_home_backup \
 && mkdir /var/jenkins_home_restore \
 && chown jenkins: /var/jenkins_home_restore

# Create Jenkins Log Folder
RUN mkdir /var/log/jenkins
RUN mkdir /var/cache/jenkins
RUN chown -R jenkins:jenkins /var/log/jenkins
RUN chown -R jenkins:jenkins /var/cache/jenkins
# switch back to jenkins user
USER jenkins
#RUN /usr/local/bin/install-plugins.sh git matrix-auth workflow-aggregator docker-workflow blueocean credentials-binding

# Set default options
ENV JAVA_OPTS="-Xmx1024m"
ENV JENKINS_OPTS="--handlerCountMax=300 --logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war"

# set a health check
#HEALTHCHECK --interval=30s \
#            --timeout=30s \
#            CMD curl -f http://127.0.0.1:8080 || exit 1

