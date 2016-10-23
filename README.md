Supervisor
==========

[![Build Status](https://travis-ci.org/jebovic/ansible-supervisor.svg?branch=master)](https://travis-ci.org/jebovic/ansible-supervisor) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.supervisor-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/supervisor)

Install and configure supervisor, you can add your own programs with yaml variables

Role Variables
--------------

```yaml
# Supervisor basic configuration
supervisor_socket: /tmp/supervisor.sock
supervisor_pidfile: /var/run/supervisord.pid
supervisor_log_path: /var/log/supervisord
supervisor_log_file: "{{ supervisor_log_path }}/supervisord.log"
supervisor_http_port: 9988
supervisor_http_username: supervisor_user
supervisor_http_password: supervisor

# Supervisor programs list, example with mailhog program
supervisor_programs:
  mailhog:
    command: /usr/local/bin/mailhog -api-bind-addr :8025 -ui-bind-addr :8025
    autostart: "true"
    autorestart: "true"
    stderr_logfile: "{{ supervisor_log_path }}/mailhog-stderr.log"
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.supervisor }
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
