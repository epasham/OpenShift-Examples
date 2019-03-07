# docker build  -t ekambaram/ocp-sonarqube -f Dockerfile .

FROM docker.io/centos:7

ENV SONAR_VERSION=6.7.4 \
    SONARQUBE_HOME=/opt/sonarqube \
    # Database configuration
    # Defaults to using H2
    SONARQUBE_JDBC_USERNAME=sonar \
    SONARQUBE_JDBC_PASSWORD=sonar \
    SONARQUBE_JDBC_URL= \
    LANG=en_US.utf8 \
    JAVA_HOME=/usr/lib/jvm/jre

# Http port
EXPOSE 9000

LABEL name="sonarqube" \
      vendor="SonarSource" \
      version="${SONAR_VERSION}-centos7" \
      io.k8s.description="Provide a SonarQube image to run on Red Hat OpenShift" \
      io.k8s.display-name="SonarQube" \
      io.openshift.build.commit.author="Ekambaram Pasham <ekambaram.pasham@gmail.com>" \
      io.openshift.expose-services="9000" \
      io.openshift.tags="sonarqube,sonar" \
      build-date="2019-03-07" \
      release="1"

RUN yum -y install --setopt=tsflags=nodocs java-1.8.0-openjdk unzip hostname && \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && cd /tmp \
    && curl -o sonarqube.zip -fSL https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip \
    && cd /opt \
    && unzip /tmp/sonarqube.zip \
    && mv sonarqube-$SONAR_VERSION sonarqube \
    && rm /tmp/sonarqube.zip*

COPY run.sh ${SONARQUBE_HOME}/bin/
ENV PATH=$PATH:${SONARQUBE_HOME}/bin
RUN useradd -l -u 10001 -r -g 0 -m -s /sbin/nologin \
        -c "sonarqube application user" sonarqube && \
    chown -R 10001:0 ${SONARQUBE_HOME} && \
    chmod -R g=u ${SONARQUBE_HOME} && \
    chmod ug+x ${SONARQUBE_HOME}/bin/run.sh

VOLUME ["${SONARQUBE_HOME}/data", "${SONARQUBE_HOME}/extensions"]

USER 10001
WORKDIR ${SONARQUBE_HOME}
ENTRYPOINT run.sh
