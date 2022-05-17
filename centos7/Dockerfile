FROM centos:7

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum -y update && \
    yum -y reinstall yum && \
    yum -y install etckeeper man && \
    etckeeper init && \
    etckeeper commit 'Initial commit'

CMD bash
