# Conditionals

## Boolean Vars

```
vars:
  epic: true
tasks:
    - shell: echo "This certainly is epic!"
      when: epic
```

## Logical AND

Can use `and` or yaml list.

```
tasks:
  - name: "do a thing"
    command: /bin/echo hello world
    when:
      - ansible_fact['some_fact'] == "root"
      - some_var == 123
  - name: "do a thing"
    command: /bin/echo hello world
    when: ansible_fact['some_fact'] and some_var == 123
```

## Logical OR

```
tasks:
  - name: "do a thing"
    command: /bin/echo hello world
    when: (ansible_fact['some_fact'] == "root") or
          (some_var == 123)
```

## Numerical Comparison

```
when: "6" | int == 6
```

## Filters and Tests

Ignore errors and trigger next task based on result

```
tasks:
  - command: /bin/echo hello world
    register: result
    ignore_errors: True
  - command: /bin/echo task failed
    when: result is failed
  - command: /bin/echo task succeeded
    when: result is succeeded
  - command: /bin/echo task skipped
    when: result is skipped
```
