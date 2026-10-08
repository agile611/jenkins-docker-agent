FROM alpine:3.7

ENV JAVA_HOME /usr/lib/jvm/default-jvm
RUN apk add --no-cache openjdk8 && \
    ln -sf "${JAVA_HOME}/bin/"* "/usr/bin/"

RUN apk add --no-cache git
RUN apk add --no-cache bash

# Add user jenkins to the image
RUN adduser -D jenkins
# Set password for the jenkins user (you may want to alter this).
RUN echo "jenkins:jenkins" | chpasswd
RUN mkdir /home/jenkins/.m2
RUN chown -R jenkins:jenkins /home/jenkins/.m2/

RUN MAVEN_VERSION=3.3.9 \
 && cd /usr/share \
 && wget -q http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz -O - | tar xzf - \
 && mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven \
 && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven

RUN apk add --no-cache openssh
RUN mkdir -p /var/run/sshd
RUN ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa
RUN ssh-keygen -f /etc/ssh/ssh_host_dsa_key -N '' -t dsa

EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
