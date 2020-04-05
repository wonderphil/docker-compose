FROM ubuntu:19.04
 
#Install basics
RUN apt-get update && apt-get install -y apt-transport-https ca-certificates wget curl gnupg-agent  openjdk-8-jre curl git
RUN apt-get install -y software-properties-common python python-pip python-setuptools

# Install Docker CLI in the agent 
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
RUN apt-get update && apt-get install -y docker-ce docker-ce-cli --allow-unauthenticated

# Get dotnet
RUN wget https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb && dpkg -i packages-microsoft-prod.deb
RUN apt-get update && apt-get install -y dotnet-sdk-3.1
 
#Install Jenkins
RUN pip install jenkins-webapi
 
# Get docker-compose in the agent container
RUN curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
RUN mkdir -p /home/jenkins
RUN mkdir -p /var/lib/jenkins
 
# Start-up script to attach the slave to the master
ADD slave.py /var/lib/jenkins/slave.py
 
WORKDIR /home/jenkins
 
ENV JENKINS_URL "http://prod-vm-docker-04:8080"
#ENV JENKINS_SLAVE_ADDRESS "jenkins_dotnet_slave"
ENV JENKINS_USER "admin"
ENV JENKINS_PASS "admin"
ENV SLAVE_NAME "jenkins_dot_net_slave"
#ENV SLAVE_SECRET ""
#ENV SLAVE_EXECUTORS "1"
ENV SLAVE_LABELS "dontnet-core"
ENV SLAVE_WORING_DIR "/home/jenkins"
ENV CLEAN_WORKING_DIR "true"
 
CMD [ "python", "-u", "/var/lib/jenkins/slave.py" ]