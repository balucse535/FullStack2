##Dockerfile is where we define the docker image and specify all the configurations required to run the app.

#Dockerfile for creating image of Alpine + mongodb + java jdk env for Fullstack2.

FROM openjdk:8u171-jdk-alpine3.8

#RUN apk add --no-cache mongodb
#RUN apt-get update && \
    apt-get install -yq --no-install-recommends wget pwgen ca-certificates && \
    apt-get install -yq --no-install-recommends libtcnative-1 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV TOMCAT_MAJOR_VERSION 8
ENV TOMCAT_MINOR_VERSION 8.0.39
ENV CATALINA_HOME /tomcat
ENV JAVA_OPTS ""
ENV TOMCAT_BIND_ON_INIT true

# INSTALL TOMCAT
RUN wget -q https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz && \
    wget -qO- https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz.md5 | md5sum -c - && \
    tar zxf apache-tomcat-*.tar.gz && \
    rm apache-tomcat-*.tar.gz && \
    mv apache-tomcat* tomcat && \
    rm -fr tomcat/webapps/examples && \
    rm -fr tomcat/webapps/docs && \
    sed -i "s/Connector port=\"8080\"/Connector port=\"8080\" bindOnInit=\"${TOMCAT_BIND_ON_INIT}\"/" tomcat/conf/server.xml

ADD create_tomcat_admin_user.sh /create_tomcat_admin_user.sh
ADD tomcatrun.sh /tomcatrun.sh
RUN chmod +x /*.sh

# The application's jar file
ARG WAR_FILE=target/TEKHealthAPI-0.0.1-SNAPSHOT.war

# Add the application's jar to the container
ADD ${WAR_FILE} /tomcat/webapps/TEKHealthAPI.war
RUN show IP
EXPOSE 8080
CMD ["/tomcatrun.sh"]




