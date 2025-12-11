FROM centos:6
MAINTAINER Jibran Patel

# Updating from Yum Repository
RUN yum -y update; yum clean all
RUN yum -y install epel-release; yum clean all 
RUN yum -y install centos-release-SCL
RUN yum -y install java-1.8*
RUN export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.121-0.b13.29.amzn1.x86_64/jre
RUN yum -y install wget
RUN yum -y install unzip

## Installing Python 2.7
RUN yum install -y python27

# Installing the MongoDB from yum repository
RUN yum -y install mongodb-server.x86_64 mongodb.x86_64
# Creating directory for db
RUN mkdir -p /data/db

# Expose port 27017 from the container to the host
EXPOSE 27017

# Downloading and Setting up apache tomcat7
RUN mkdir -p /apps/tomcat/tomcat7
RUN cd /apps/tomcat/tomcat7
RUN wget http://www-eu.apache.org/dist/tomcat/tomcat-7/v7.0.75/bin/apache-tomcat-7.0.75.zip
RUN unzip apache-tomcat-7.0.75.zip
RUN chmod 755 -R apache-tomcat-7.0.75/bin/
RUN export CATALINA_HOME=/apps/tomcat/tomcat7/apache-tomcat-7.0.75
RUN export CATALINA_BASE=$CATALINA_HOME
RUN apache-tomcat-7.0.75/bin/startup.sh

