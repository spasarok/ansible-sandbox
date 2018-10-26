# [Facts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#information-discovered-from-systems-facts)

Facts are discoverable information injected into your systems.

* Usually injected under the `ansible_` prefix
* Get all facts with `ansible hostname -m setup`
* Reference facts with yaml dictionary syntax
  * ex. `{{ ansible_facts['devices']['sda']['model'] }}`
* Can gather facts from remote hosts
  * ex. `{{ hostvars['asdf.example.com']['ansible_facts']['os_family'] }}`
* Facts expire at end of playbook run

## Fact Caching

Can cache facts from remote machines to use in subsequent plays.

* Will need to change the gathering setting to `smart` or `explicit` or set `gather_facts` to `False` in most plays
* Can use `redis` or `jsonfile` caching plugins (view link for details)

## Disabling Fact Injection

```
- hosts: whatever
  gather_facts: no
```

## Local (Custom) Facts

Can create custom facts on a remote machine.

* Location
  * Defaults to `/etc/ansible/facts.d/{{file_name}}.fact`
  * Can specify alternate directory with `fact_path` in the play
* Valid file formats
  * JSON
  * INI
  * Executable file returning JSON
* Notes
  * Keys will be converted to lowercase

### Example

Facts file located at `/etc/ansible/facts.d/preferences.fact`.

```
# /etc/ansible/facts.d/preferences.fact

[general]
asdf=1
bar=2
```

Resulting fact, found with `ansible <hostname> -m setup -a "filter=ansible_local"`.

```
"ansible_local": {
        "preferences": {
            "general": {
                "asdf" : "1",
                "bar"  : "2"
            }
        }
 }
```

Accessing local facts from a playbook.

```
{{ ansible_local['preferences']['general']['asdf'] }}
```

Must rerun setup module to inject local facts copied from a remote machine into the current play. (Otherwise, the copied facts will be available in the next play that gathers fact information.)

```
- hosts: webservers
  tasks:
    - name: create directory for ansible custom facts
      file: state=directory recurse=yes path=/etc/ansible/facts.d
    - name: install custom ipmi fact
      copy: src=ipmi.fact dest=/etc/ansible/facts.d
    - name: re-read facts after adding custom fact
      setup: filter=ansible_local
```
