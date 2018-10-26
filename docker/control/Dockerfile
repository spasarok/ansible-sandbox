FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y \
  ansible \
  man \
  openssh-server \
  vim

RUN mkdir -p /etc/ansible/group_vars \
             /etc/ansible/host_vars \
             /etc/ansible/library \
             /etc/ansible/module_utils \
             /etc/roles/common

RUN echo force rebuild

# Never do this with real keys
COPY ./docker/control/id_rsa /root/.ssh/id_rsa
COPY ./docker/control/id_rsa.pub /root/.ssh/id_rsa.pub

COPY ./docker/control/ansible/hosts /etc/ansible/hosts
COPY ./docker/control/ansible/ansible.cfg /etc/ansible/ansible.cfg

CMD tail -f /dev/null
