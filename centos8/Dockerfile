FROM centos:8

RUN dnf -y update && \
    dnf -y reinstall dnf yum && \
    dnf -y install man

RUN dnf -y install git
RUN git config --global user.name 'root' && \
    git config --global user.email 'root@localhost' && \
    cd /etc && \
    git init && \
    git add -A . && \
    git commit -m 'Initial commit'

RUN echo 'set -o vi' >> '/root/.bashrc'
RUN echo 'alias ls="ls --color"' >> '/root/.bashrc'

CMD bash
