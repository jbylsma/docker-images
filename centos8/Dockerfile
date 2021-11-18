FROM centos:8

RUN dnf -y install dnf-plugins-core && \
    dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && \
    dnf -y update && \
    dnf -y reinstall dnf yum && \
    dnf -y install etckeeper etckeeper-dnf man && \
    etckeeper init && \
    etckeeper commit 'Initial commit'

CMD bash
