rsyslog
=======

Redirect all logs to one or more log servers. Choose between UDP or TCP, defaults to UDP. Will drop all logs lower than EMERG/PANIC if connection is interrupted and queue gets full.

Important! Sends logs unencrypted to remote syslog server.

Version
-------

* `1.2.0` --- remove ubuntu precise from testing
* `1.1.1` --- fix lint warnings
* `1.1.0` --- added ubuntu focal, 20.04
* `1.0.6` --- tested with Ansible 2.9.11
* `1.0.5` --- prepare for github
* `1.0.4` --- bugfix, error when running in check mode
* `1.0.3` --- install rsyslog even in check mode
* `1.0.2` --- bugfix, allow running Ansible in check mode
* `1.0.1` --- fixed missing default
* `1.0.0` --- initial role
* `master` --- latest development version

Requirements
------------

This role is limited to

* Ubuntu 20.04 - Bionic
* Ubuntu 18.04 - Bionic
* Ubuntu 16.04 - Xenial
* Ubuntu 14.04 - Trusty
* CentOS 8
* CentOS 7
* CentOS 6

Role Variables
--------------

* `rsyslog_journald_size` --- `1G`
* `rsyslog_config` --- list of dicts configuring syslog servers - see below for dictionary keywords, default `[]`
    * `type` --- syslog type - defaults to forward, default `omfwd`
    * `resume_retry_count` --- number of retries before loosing data, default `-1`
    * `queue_type` --- which kind of queue to use, default `LinkedList`
    * `queue_size` --- max size of the queue, default `10000`
    * `queue_save_on_shutdown` --- save state of queue on shutdown, default `true`
    * `target_ip` --- destination syslog server, __required__
    * `target_port` --- destination syslog port, default `514`
    * `target_protocol` --- protocol to use when talking to syslog server, default `udp`

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: rsyslog
          rsyslog_config:
          - target_ip: 10.100.10.10
            target_protocol: udp

queue.discardseverity=”8”)
          

Testing
-------

### Test environment for all OSes

```bash
cd tests
vagrant up
```

Log into the syslog server and look at the logs.

```bash
vagrant ssh syslog
cd /var/log
find
```

### Rerun role

Run role on all OSes again.

```bash
vagrant provision
```

### Debug interactively

This uses cluster ssh to work with all vagrant boxes at the same time.

```bash
vagrant ssh-config > ~/.ssh/config
cat ~/.ssh/config | grep ^Host | cut -d\  -f2 | xargs cssh
```

License
-------

GPLv2

Author Information
------------------

Created 2020 by [Arnulf Heimsbakk](mailto:arnulf.heimsbakk@met.no) for MET Norway.

###### set vim: spell spelllang=en:
