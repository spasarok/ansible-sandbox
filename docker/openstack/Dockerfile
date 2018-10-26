FROM ubuntu:18.04
EXPOSE 22

RUN apt-get update
RUN apt-get install -y \
    man \
    openssh-server \
    python \
    python-pip \
    supervisor \
    vim \
    python-pip

COPY ./docker/openstack/requirements.txt /root/requirements.txt
RUN pip install -r /root/requirements.txt

COPY ./docker/control/id_rsa.pub /root/.ssh/authorized_keys

CMD service ssh start && tail -f /dev/null
