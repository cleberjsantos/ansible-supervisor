supervisor
==========

[![Ansible Role](https://img.shields.io/ansible/role/6078.svg)](https://galaxy.ansible.com/list#/roles/6078)
[![Travis CI](https://img.shields.io/travis/cleberjsantos/ansible-supervisor/master.svg)](http://travis-ci.org/cleberjsantos/ansible-supervisor)
[![Tag](https://img.shields.io/github/tag/cleberjsantos/ansible-supervisor.svg)]()


Installs and configures supervisor process control system. Ansible role which manage [supervisor](http://supervisord.org)

* Install and manage [supervisor](http://supervisord.org)
* Install [superlance](http://superlance.readthedocs.org)
* Manage supervisor tasks
* Provide handlers for reload and restart supervisor

Requirements
------------

* This role requires Ansible 1.2 or higher.


Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml).

    supervisor_enabled: yes                   # The role is enabled
    supervisor_version: "3.1.3"
    supervisor_bindir: "/usr/local/bin"
    supervisor_bin: "{{ supervisor_bindir }}/supervisord"
    supervisor_pid: /var/run/supervisord.pid
    supervisor_nofile: 65356                  # Set max opened files (set blank to default limits)
    supervisor_cfgdir: /etc/supervisor        # path to config directory
    supervisor_conf_file: "{{ supervisor_cfgdir }}/supervisord.conf"
    supervisor_logdir: /var/log/supervisor    # path to logs directory
    supervisor_incdir: "{{supervisor_cfgdir}}/conf.d" # path to include directory
    supervisor_tasks: []                      # List of supervisor programs
                                              # Ex. supervisor_tasks:
                                              #       - name: <name>
                                              #         option: value
                                              #         option: value
                                              #         option: value
    supervisor_events: []                     # similar to tasks/programs but for eventlisteners like crashmail
    supervisor_groups: []                     # groups of tasks
    supervisor_superlance: no                 # Install superlance (http://superlance.readthedocs.org/

    supervisor_username: admin                # The username required for authentication to this HTTP server
    supervisor_password: mypass               # The password required for authentication to this HTTP server
    supervisor_port: '127.0.0.1:9001'         # A TCP host:port value on which supervisor will listen for HTTP/XML-RPC requests
    supervisor_serverurl: 'unix:///var/run/supervisor.sock' # The URL passed in the environment to the subprocess process as SUPERVISOR_SERVER_URL


Example Playbook
-----------------

Example of usage:

    - hosts: servers
      roles:
        - { role: cleberjsantos.supervisor, tags: [supervisor, management] }


Custom variables sample:
------------------------

Eg. you can add custom variables in the group_vars or host_vars. 

    ---
    # file: /etc/ansible/group_vars/all
    
    supervisor_tasks:
        - name: ping
          command: ping google.com
          autostart: true
          autorestart: true
    supervisor_events:
        - name: memmon
          command: memmon -p program=100MB
          events: TICK_60
          process_name: memmon
    supervisor_groups:
        - name: my_group
          programs: ping


Eg 2 with support for environment option.

    ---
    # file: /etc/ansible/group_vars/all
    
    supervisor_tasks:
      - name: phpWorker
        environment:
          MYSQL_HOST: "127.0.0.1"
          MYSQL_PORT: 3306
        command: php worker.php


    # generates inside supervisord.conf
    
    [program:phpWorker]
    environment= MYSQL_HOST="127.0.0.1", MYSQL_PORT="3306",
    command=php worker.php


License
-------

GPLv2

Author Information
------------------

Cleber J Santos
