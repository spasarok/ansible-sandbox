# [Configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html#intro-configuration)

* Methods (checked in this order)
  * Command line argument (precedence over environment variables)
  * Environment variable (precedence over file)
  * File (checked in this order)
    * `ANSIBLE_CONFIG` can store path to configuration file
    * `ansible.cfg` in current directory
    * `~/.ansible.cfg` in home directory
    * `/etc/ansible/ansible.cfg`
* Security
  * Configuration file should not be world writable
  * Not loaded from working directory if world writable
  * Can get around this with `ANSIBLE_CONFIG`
* [Configuration Settings](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#ansible-configuration-settings)
