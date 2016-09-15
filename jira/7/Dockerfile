FROM centos:7
MAINTAINER Ben Wu <wucheokman@gmail.com>

ENV CONF_HOME     /var/atlassian/jira
ENV CONF_INSTALL  /opt/atlassian/jira
ENV CONF_VERSION  7.2.1

RUN yum -y update && yum -y install epel-release \
    && yum -y install tar \
    git \
    wget \
    xmlstarlet \
    java-1.8.0-openjdk \
    && yum clean all \
    && mkdir -p                "${CONF_HOME}" \
    && chmod -R 700            "${CONF_HOME}" \
    && chown daemon:daemon     "${CONF_HOME}" \
    && mkdir -p                "${CONF_INSTALL}/conf" \
    && curl -Ls                "https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-${CONF_VERSION}.tar.gz" | tar -xz --directory "${CONF_INSTALL}" --strip-components=1 --no-same-owner \
    && curl -Ls                "http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.36.tar.gz" | tar -xz --directory "${CONF_INSTALL}/lib" --strip-components=1 --no-same-owner "mysql-connector-java-5.1.36/mysql-connector-java-5.1.36-bin.jar" \
    && chmod -R 700            "${CONF_INSTALL}/conf" \
    && chmod -R 700            "${CONF_INSTALL}/temp" \
    && chmod -R 700            "${CONF_INSTALL}/logs" \
    && chmod -R 700            "${CONF_INSTALL}/work" \
    && chown -R daemon:daemon  "${CONF_INSTALL}/conf" \
    && chown -R daemon:daemon  "${CONF_INSTALL}/temp" \
    && chown -R daemon:daemon  "${CONF_INSTALL}/logs" \
    && chown -R daemon:daemon  "${CONF_INSTALL}/work" \
    && sed -i -- 's/grep/egrep/g' ${CONF_INSTALL}/bin/check-java.sh \
    && sed -i -- 's/"java version"/"(openjdk|java) version"/g' ${CONF_INSTALL}/bin/check-java.sh

USER daemon:daemon
EXPOSE 8080

# Set the default working directory as the Confluence home directory.
WORKDIR ${CONF_HOME}
ENV JIRA_HOME ${CONF_HOME}
ENV _RUNJAVA 'java'

# Run Atlassian Confluence as a foreground process by default.
CMD ["/opt/atlassian/jira/bin/start-jira.sh", "-fg"]

