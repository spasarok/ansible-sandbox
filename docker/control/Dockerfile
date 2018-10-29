FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y \
  ansible \
  man \
  openssh-server \
  vim

# Never do this with real keys
COPY ./docker/control/id_rsa /root/.ssh/id_rsa_fake
COPY ./docker/control/id_rsa.pub /root/.ssh/id_rsa_fake.pub
COPY ./docker/control/ssh_config /root/.ssh/config

RUN echo force rebuild

RUN mkdir -p /etc/ansible/group_vars/all \
             /etc/ansible/host_vars \
             /etc/ansible/library \
             /etc/ansible/module_utils \
             /etc/ansible/playbooks \
             /etc/ansible/roles/common
 COPY ./ansible/hosts /etc/ansible/hosts
 COPY ./ansible/ansible.cfg /etc/ansible/ansible.cfg

CMD tail -f /dev/null
