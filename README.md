# Ansible MySQL Tests

This project dockerizes an Ansible control machine and managed nodes for the purpose of studying Ansible from a personal machine. This project is NOT secure and should not be used outside of a local test environment.

# Requirements

* Docker Compose

# Setup

Set permissions for SSH keys so that they're usable. This only needs to be done once.

```
chmod 700 setup.sh
./setup.sh
```

# Docker

## Build

Build the `ansible_control` and `compute` images.

```
docker-compose build
```

## Run

Start the `ansible_control` and `compute_[1:3]` containers.

```
docker-compose up
```

## Stop

Stop running containers. Do not clear persistent storage (volumes).

```
docker-compose down
```

Stop running containers and clear persistent storage.

```
docker-compose down -v
```

## Images

### Control Machine

* Uses Ubuntu 18.04
* Dummy RSA keys copied to `/root/.ssh` at build time
  * DO NOT DO THIS IN PROD

### Compute Nodes

* Uses MySQL 8.0
* Uses `supervisor` to start `sshd` and `mysqld`
* Root MySQL password is `password`
* MySQL config (and password) copied to `/root/.my.cnf` at build time
  * DO NOT DO THIS IN PROD

# Tinker

## Local Commands

Connect to control machine container in terminal.

```
docker exec -it ansible_control /bin/bash
```

Connect to node container in terminal.

```
docker exec -it compute_1 /bin/bash
```

Connect to mysql running on compute from personal machine.

```
mysql -u root -h 127.0.0.1 -P 3307 -p # compute_1
mysql -u root -h 127.0.0.1 -P 3308 -p # compute_2
mysql -u root -h 127.0.0.1 -P 3309 -p # compute_3
```

## Example Control Commands

Docker exec into `control_machine` container.

```
$ docker exec -it control_machine /bin/bash
root@79c43de9ac09:/#
```

SSH into a compute node.

```
root@79c43de9ac09:/# ssh root@compute_1
```

Ping all nodes.

```
root@79c43de9ac09:/# ansible all -m ping
localhost | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
compute_2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
compute_3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
compute_1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Ping a specific node.

```
root@c5f9e5557427:/# ansible compute_1 -m ping
compute_1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Run a playbook.

```
root@c5f9e5557427:/# ansible-playbook /root/playbooks/mysql.yml
```

# MySQL

[Example Commands](https://dev.mysql.com/doc/refman/5.5/en/creating-tables.html)

```
# From local machine

$ brew services start mysql
$ mysql
$ mysql> select User from mysql.user;
$ mysql> show databases;
$ mysql> use test_db;
$ mysql> create table test_table (test_column varchar(20));`
$ mysql> show tables;
$ mysql> describe test_table;
$ exit
$ mysql -u root -h 127.0.0.1 -P 3307 -e "USE test_db; CREATE TABLE test_table_2 (test_column VARCHAR(20));"
```

## Notes

* MySQL root password is empty by default
* `MYSQL_PWD` is [insecure](https://dev.mysql.com/doc/refman/8.0/en/password-security-user.html) and should not be used in production environments
* Passing `MYSQL_PWD` as environment variable prevents MySQL image from starting properly
