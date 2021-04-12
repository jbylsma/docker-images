FROM registry.access.redhat.com/ubi8/ubi:8.1

# Update system for baseline packages (might be the wrong way to go about it)
RUN dnf --disableplugin=subscription-manager -y update

# Add documentation for baseline repos
RUN dnf --disableplugin=subscription-manager -y reinstall dnf yum
RUN dnf --disableplugin=subscription-manager -y install man

# Fake etckeeper
RUN dnf --disableplugin=subscription-manager -y install git
RUN git config --global user.email "root@localhost" && git config --global user.name 'root'
RUN cd /etc && git init && git add -A . && git commit -m 'Initial commit'

RUN echo 'set -o vi' >> '/root/.bashrc'
RUN echo 'alias ls="ls --color"' >> '/root/.bashrc'
RUN echo 'alias dnf="dnf --disableplugin=subscription-manager"' >> '/root/.bashrc'
RUN echo 'alias yum="yum --disableplugin=subscription-manager"' >> '/root/.bashrc'

CMD bash
