FROM mysql:8.0.12
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

# Never do this with real passwords
COPY ./docker/mysql/.my.cnf /root/.my.cnf
COPY ./docker/control/id_rsa.pub /root/.ssh/authorized_keys

RUN mkdir -p /var/log/supervisor \
             /run/sshd
COPY ./docker/mysql/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD supervisord
