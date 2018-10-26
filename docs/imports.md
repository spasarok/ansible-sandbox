# [Imports](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html)

* Imports
  * `import_*` prefix is static
  * `include_*` prefix is dynamic
  * `include` task alone is now deprecated

## Static Imports

```
import_*
```

* Preprocessed when playbook is parsed
* Parent task options are copied to all child tasks contained within the import
* `roles` imports roles statically

### Limitations

* Loops cannot be used with imports at all
* When using variables for the target file or role name, variables from inventory sources (host/group vars, etc.) cannot be used

## Dynamic Imports

```
include_*
```

* Processed at runtime when task starts
* Primarily used for looping
* Parent task options only apply to the dynamic task as it is evaluated and are not be copied to child tasks
* `include_roles` imports roles dynamically

### Limitations

* Tags which only exist inside a dynamic include will not show up in `--list-tags` output
* You cannot use notify to trigger a handler name which comes from inside a dynamic include
* You cannot use `--start-at-task` to begin execution at a task inside a dynamic include

## Task Files

* Can import tasks and handlers to playbook from task files
* Can include variables in the import

### Task File

```
- name: hello
  command: /bin/echo hello
- name: world
  command: /bin/echo world
- name: echo
  command: /bin/echo {{ first_word }} {{ second_word }}
```

### Handler File

```
- name: restart apache
  service:
    name: apache
    state: restarted
```

### Playbook

```
- hosts: servers
  remote_user: root
  handlers:
    - import_tasks: static_handlers.yml
    - include_tasks: dynamic_handlers.yml
  tasks:
    - import_tasks: static_tasks.yml
    - include_tasks: dynamic_tasks.yml
    - include_tasks: parameterized_tasks.yml
      vars:
        first_word: hello
        second_word: world
```
