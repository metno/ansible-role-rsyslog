rsyslog
=======

Redirect all logs to one or more log servers. Choose between UDP or TCP, defaults to UDP. Will drop all logs lower than EMERG/PANIC if connection is interrupted and queue gets full.

Important! Sends logs unencrypted to remote syslog server.

Version
-------

* `3.1.0` --- Add support for custom templates
* `3.0.0` --- Add Unsible-core 2.16. Removed support for Ubuntu xenial and bionic
* `2.2.0` --- Support Ubuntu 24.04
* `2.1.1` --- Allow Fedora CoreOS 39
* `2.1.0` --- Initial support for Fedora CoreOS, but with no tests
* `2.0.1` --- bug fix, ansible-lint
* `2.0.0` --- updated ansible to 2.12.9
* `1.5.0` --- add RHEL9 and CentOS Stream 8 support
* `1.4.0` --- add jammy support; remove centos8 support
* `1.3.0` --- add rhel8 support; remove trusty and centos6 support
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

* Ubuntu 24.04 - Noble
* Ubuntu 22.04 - Jammy
* Ubuntu 20.04 - Focal
* CentOS 7
* CentOS Stream 8
* RHEL 8
* RHEL 9
* Fedora CoreOS 38
* Fedora CoreOS 39

Role Variables
--------------

* `rsyslog_journald_size` --- `1G`
* `rsyslog_config` --- list of dicts configuring syslog servers - see below for dictionary keywords, default `[]`
    * `type` --- syslog type - defaults to forward, default `omfwd`
    * `template` --- sets a custom default template for this module, default depends on the module
    * `resume_retry_count` --- number of retries before loosing data, default `-1`
    * `queue_type` --- which kind of queue to use, default `LinkedList`
    * `queue_size` --- max size of the queue, default `10000`
    * `queue_save_on_shutdown` --- save state of queue on shutdown, default `true`
    * `target_ip` --- destination syslog server, __required__
    * `target_port` --- destination syslog port, default `514`
    * `target_protocol` --- protocol to use when talking to syslog server, default `udp`

Dependencies
------------

The RHEL8 image needs to be registered with RedHat to install packages.

Example Playbook
----------------

Note that `- target_ip: ...` can be repeated

    - hosts: servers
      roles:
        - role: rsyslog
          rsyslog_config:
          - target_ip: 10.100.10.10
            target_protocol: udp
          - target_ip: 10.100.10.11
            target_protocol: udp

Testing
-------

NOTICE: Fedora CoreOS is tested manually, but currently no automatic tests
are added for FCOS.

To test RHEL8 with vagrant, install `vagrant-register`

```bash
vagrant plugin install vagrant-registration
```

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

Created 2020 by IT Infrastructure at MET Norway

Contactpoint: [IT Infrastructure Basis Team](mailto:it-is-basis@met.no)


###### set vim: spell spelllang=en:
