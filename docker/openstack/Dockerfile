FROM ubuntu:18.04
EXPOSE 22

RUN apt-get update
RUN apt-get install -y \
    iputils-ping \
    man \
    netcat \
    openssh-server \
    python \
    python-pip \
    supervisor \
    vim

RUN pip install ansible

COPY ./docker/openstack/requirements.txt /root/requirements.txt
RUN pip install -r /root/requirements.txt
RUN pip install \
    openstacksdk==0.17.2 \
    python-barbicanclient==4.7.0 \
    python-cinderclient==4.0.1 \
    python-glanceclient==2.12.1 \
    python-keystoneclient==3.17.0 \
    python-neutronclient==6.0.0 \
    python-novaclient==11.0.0 \
    python-openstackclient==3.16.1

COPY ./docker/control/id_rsa.pub /root/.ssh/authorized_keys

CMD service ssh start && \
    tail -f /dev/null
