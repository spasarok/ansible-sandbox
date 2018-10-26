FROM ubuntu:18.04

RUN apt-get update
RUN apt-get install -y \
  ansible \
  man \
  openssh-server \
  vim

# Never do this with real keys
COPY ./docker/control/id_rsa /root/.ssh/id_rsa
COPY ./docker/control/id_rsa.pub /root/.ssh/id_rsa.pub

CMD tail -f /dev/null
