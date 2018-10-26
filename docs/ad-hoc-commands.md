# [Ad Hoc Commands](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)

* Parallelism
  * Can run Ansible commands on multiple hosts in parallel
* Quotes in ad hoc commands
  * Single quotes evaluate terms on managed node
    * `'echo $TERM'`
  * Double quotes evaluate terms on control machine
    * `"echo $TERM"`
* File transfer
  * Transfer files with `copy` module
  * Change ownership with `file` module
  * Change permission with `file` module
  * Create and delete with `file` module
* Modules and module installation (system packages)
  * Ensure that package is installed but don't update, install if not
    * `ansible {{group_name}} -m apt -a "name=apache state=present"`
  * Ensure that package is installed to a specific version, install to version if not
    * `ansible {{group_name}} -m apt -a "name=apache2=2.4.29-1ubuntu4.3 state=present"`
  * Ensure that package is installed to latest version, install to latest if not
    * `ansible {{group_name}} -m apt -a "name=apache2 state=latest"`
  * Ensure that package is not installed, uninstall if installed (process can't be running)
    * `ansible {{group_name}} -m apt-get -a "name=apache2 state=absent"`
* User creation and deletion
  * `$ ansible all -m user -a "name=foo password=<encrypted password here>"`
  * `$ ansible all -m user -a "name=foo state=absent"`
* Deploy from source control
* Managing services
  * Ensure service is running, start if not
    * `$ ansible {{group_name}} -m service -a "name=apache2 state=started"`
  * Restart a running service
    * `ansible {{group_name}} -m service -a "name=apache2 state=restarted"`
  * Ensure a service is stopped, stop if not
    * `$ ansible {{group_name}} -m service -a "name=httpd state=stopped"`
* Timeouts for long-running operations
* Gathering facts
  * Facts are discoverable variables about a system
  * `$ ansible all -m setup`
