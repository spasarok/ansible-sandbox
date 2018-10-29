FROM ubuntu:18.04
EXPOSE 22

RUN apt-get update
RUN apt-get install -y \
    man \
    openssh-server \
    python \
    python-pip \
    supervisor \
    vim

RUN pip install ansible

COPY ./docker/control/id_rsa.pub /root/.ssh/authorized_keys

CMD service ssh start && \
    tail -f /dev/null
