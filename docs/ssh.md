# SSH Basics

* Control machine's public key goes in `~/.ssh/authorized_keys` of managed nodes
* Host checking
  * Set `host_key_checking = False` to prevent interactive SSH prompt when target node has not been added to `known_hosts`
