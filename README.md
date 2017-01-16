Supervisor
==========

[![Build Status](https://travis-ci.org/jebovic/ansible-supervisor.svg?branch=master)](https://travis-ci.org/jebovic/ansible-supervisor) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.supervisor-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/supervisor)

Install and configure supervisor, you can add your own programs with yaml variables

This role is a part of my [OPS project](https://github.com/jebovic/ops), follow this link to see it in action. OPS provides a lot of stuff, like a vagrant file for development VMs, playbooks for roles orchestration, inventory files, examples for roles configuration, ansible configuration file, and many more.

Compatibility
-------------

Tested and approved on :

* Debian jessie (8+)
* Ubuntu Trusty (14.04 LTS)
* Ubuntu Xenial (16.04 LTS)

Role Variables
--------------

```yaml
# Supervisor install ocnfiguration
supervisor_packages:
  - supervisor
supervisor_pip_packages:
  - superlance

# Supervisor basic configuration
supervisor_socket: /tmp/supervisor.sock
supervisor_pidfile: /var/run/supervisord.pid
supervisor_log_path: /var/log/supervisord
supervisor_log_file: "{{ supervisor_log_path }}/supervisord.log"
supervisor_http_host: "{{ ansible_host }}"
supervisor_http_port: 9988
supervisor_http_username: supervisor_user
supervisor_http_password: supervisor

# Supervisor programs
supervisor_programs: []

# Supervisor programs
supervisor_events: []
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.supervisor }
```

Example : config
----------------

```yaml
# supervisor UI ip & port bindig
supervisor_http_host: 127.0.0.1
supervisor_http_port: 9988
# supervisor programs configuration
supervisor_programs:
  traefik:
    command: "{{ traefik_bin_path }} -c {{ traefik_config_dir }}/traefik.toml"
    autostart: "true"
    autorestart: "true"
    stderr_logfile: "{{ supervisor_log_path }}/traefik-stderr.log"
    stderr_logfile_maxbytes: 1MB
    stderr_logfile_backups: 10
    stdout_logfile: "{{ supervisor_log_path }}/traefik-stdout.log"
    stdout_logfile_maxbytes: 1MB
    stdout_logfile_backups: 10
  consul:
    command: "{{ consul_bin_path }} agent -ui -bind={{ ansible_host }} -client=0.0.0.0 -node={{ ansible_fqdn }} -bootstrap -server -http-port {{ consul_http_port }} -data-dir={{ consul_data_dir }} -config-dir={{ consul_config_dir }} -domain={{ ansible_fqdn }}."
    autostart: "true"
    autorestart: "true"
    stderr_logfile: "{{ supervisor_log_path }}/consul-stderr.log"
    stderr_logfile_maxbytes: 1MB
    stderr_logfile_backups: 10
    stdout_logfile: "{{ supervisor_log_path }}/consul-stdout.log"
    stdout_logfile_maxbytes: 1MB
    stdout_logfile_backups: 10
  mailhog:
    command: /usr/local/bin/mailhog -api-bind-addr :8025 -ui-bind-addr :8025
    autostart: "true"
    autorestart: "true"
    stderr_logfile: "{{ supervisor_log_path }}/mailhog-stderr.log"

# supervisor events configuration (depend on superlance plugin installation)
supervisor_events:
  httpok:
    command: httpok -p mailhog http://localhost:8025
    events: TICK_60
```

Tags
----

* supervisor_config : only update config and restart service

License
-------

MIT

Author Information
------------------

Jérémy Baumgarth https://github.com/jebovic
