# [Patterns](https://docs.ansible.com/ansible/latest/user_guide/intro_patterns.html#intro-patterns)

* `ansible <pattern_goes_here> -m <module_name> -a <arguments>`
* Target all hosts
  * `all`
  * `*`
* Target specific host or set of hosts
  * `one.example.com`
  * `one.example.com:two.example.com`
  * `192.0.2.50`
  * `192.0.2.*`
* Target a group
  * `webservers`
* OR
  * `webservers:dbservers`
* Exclusion
  * `webservers:!phoenix`
* Intersection
  * `webservers:&staging`
* Combinations
  * `webservers:dbservers:&staging:!phoenix`
* Variables
  * `webservers:!{{excluded}}:&{{required}}`
* Wildcards
  * `*.example.com`
  * `*.com`
* Positional
  * `webservers[0]`     
  * `webservers[-1]`
  * `webservers[0:2]`
  * `webservers[1:]`
  * `webservers[:3]`
* Regex
  * `~(web|db).*\.example\.com`
