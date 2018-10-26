# [Inventory](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

* Inventory file
  * Stored at `/etc/ansible/hosts`
  * Can also specify file with `-i <path>`
  * INI or yaml
* Aliases
  * `jumper ansible_port=5555 ansible_host=192.0.2.50`
* Groups
  * Can group similar nodes under a common label
* Default Ansible groups
  * `all` and `ungrouped`
  * Every host belongs to at least two groups
* Group ranges
  * `www[01:50].example.com`
  * `db-[a:f].example.com`
* Groups of groups
  * Use label `[{{group_name}}:children]`
* Group variables
  * Can put in inventory file under label `[{{group_name}}:vars]`
  * Can put in
* Variable files
  * **The preferred practice in Ansible is to not store variables in the main inventory file.**
  * Can put variables in `group_vars/{{group_name}}` and `host_vars/{{host_name}}` in playbook directory or inventory directory
    * Playbook directory takes precedence
    * Files must be yaml or json
  * Example
    * `/etc/ansible/host`
    * `/etc/ansible/group_vars/{{group_name}}`
    * `/etc/ansible/group_vars/{{other_group_name}}`
    * `/etc/ansible/host_vars/{{host_name}}`
  * Can use group directory if single group needs more than one variable file
    * `/etc/ansible/host`
    * `/etc/ansible/group_vars/{{group_name}}/{{file_name}}`
* Group precedence
  * By default, group rules will be applied alphabetically for hosts belonging to multiple groups
  * Can explicitly set group priority with `ansible_group_priority`
* Connection types
  * SSH
  * Local
  * Docker
