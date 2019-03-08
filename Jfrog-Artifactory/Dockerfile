# docker build  -t ekambaram/ocp-artifactory -f Dockerfile .

# Base Image Name
FROM docker.io/centos:7

# Docker Image labels
LABEL name="JFrog OSS Repository Manager" \            
 version="5.3.1" \
 build-date="2019-03-08" \
 release="1"
            
# Kubernetes Labels
LABEL io.k8s.description="JFrog OSS Repository Manager" \
 io.k8s.display-name="JFrog OSS Repository Manager" 

# OpenShift Labels 
LABEL io.openshift.build.commit.author="Ekambaram Pasham <ekambaram.pasham@gmail.com>" \
 io.openshift.tags="jfrog,artifactory" 

# Environment variables
ENV ARTIFACTORY_VERSION 5.3.1 \
 ARTIFACTORY_HOME /opt/artifactory

# Install binaries
RUN yum -y install --setopt=tsflags=nodocs java-1.8.0-openjdk-devel.x86_64 maven lsof curl tar unzip \
 && yum clean all \
 && rm -rf /var/cache/yum \
 && cd /tmp \
 && curl -o artifactory.zip -fSL https://bintray.com/artifact/download/jfrog/artifactory/jfrog-artifactory-oss-$ARTIFACTORY_VERSION.zip \
 && cd /opt \ 
 && unzip /tmp/artifactory.zip \
 && mv /opt/artifactory-oss-${ARTIFACTORY_VERSION} artifactory \
 && rm /tmp/artifactory.zip


#RUN groupadd -r artifactory -g 1001 \
# && useradd -u 1001 -r -g artifactory -d /opt/artifactory -s /sbin/nologin -c "artifactory user" artifactory
#RUN chown -R artifactory:0 ${ARTIFACTORY_HOME}
#RUN chmod 774 -R ${ARTIFACTORY_HOME}

RUN useradd -l -u 10001 -r -g 0 -m -s /sbin/nologin \
 -c "artifactory user" artifactory \
 && chown -R 10001:0 ${ARTIFACTORY_HOME} \
 && chmod -R g=u ${ARTIFACTORY_HOME} \
 && chmod ug+x ${ARTIFACTORY_HOME}/bin/artifactory.sh

USER 1001

EXPOSE 8081

VOLUME /opt/artifactory/etc

WORKDIR $ARTIFACTORY_HOME

CMD ["/opt/artifactory/bin/artifactory.sh"]
