FROM redhat/ubi8-init

RUN rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && \
    dnf -y update && \
    dnf -y install etckeeper etckeeper-dnf patch && \
    etckeeper init && \
    etckeeper commit 'Initial commit'

RUN dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm && \
    dnf install -y postgresql14-server
