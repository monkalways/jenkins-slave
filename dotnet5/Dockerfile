FROM openshift/jenkins-slave-base-centos7

# Labels consumed by Red Hat build service
LABEL com.redhat.component="rh-dotnet5-jenkins-slave-docker" \
    name="dotnet/dotnet-5-jenkins-slave-centos7" \
    version="5.0" \
    architecture="x86_64" \
    release="1" \
    io.k8s.display-name="Jenkins Slave .NET Core 5.0" \
    io.k8s.description="The jenkins slave dotnet image has the dotnet tools on top of the jenkins slave base image." \
    io.openshift.tags="openshift,jenkins,slave,dotnet,dotnet5"

# Don't download/extract docs for nuget packages
# Enable nodejs and dotnet scl
ENV DOTNET_CORE_VERSION=5.0 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    ENABLED_COLLECTIONS="rh-nodejs10 dotnet-sdk-5.0" \
    NUGET_XMLDOC_MODE=skip

COPY contrib/bin/scl_enable /usr/local/bin/scl_enable

COPY .gitconfig /home/jenkins/.gitconfig

RUN rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm

# Install
RUN yum install -y centos-release-dotnet centos-release-scl-rh && \
    INSTALL_PKGS="dotnet-sdk-5.0 rh-nodejs10-npm" && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    # yum cache files may still exist (and quite large in size)
    rm -rf /var/cache/yum/*

# Trigger first time actions.
# RUN scl enable dotnet-sdk-5.0 'dotnet help'


RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001

# RUN dotnet tool install --global dotnet-sonarscanner 