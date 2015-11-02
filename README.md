Ansible Role: carbon-relay-ng
=============================

[![Build Status](https://travis-ci.org/redouane/ansible-role-carbonrelayng.svg?branch=master)](https://travis-ci.org/redouane/ansible-role-carbonrelayng)

Builds, Installs and configures carbon-relay-ng

Requirements
------------

- git

Role Variables
--------------

```yaml

carbonrelayng_golang_workspace: /tmp/go
carbonrelayng_install_dir: /opt/carbon-relay-ng

carbonrelayng_listen_addr: 0.0.0.0:2003
carbonrelayng_admin_addr: 0.0.0.0:2004
carbonrelayng_http_addr: 0.0.0.0:8081
carbonrelayng_spool_dir: /var/spool/carbon-relay-ng
carbonrelayng_pid_file: "" #/var/run/carbon-relay-ng.pid
carbonrelayng_log_level: notice
carbonrelayng_badmetrics_max_usage: 24h
carbonrelayng_init_commands:
- addBlack prefix collectd.localhost  # ignore hosts that don't set their hostname properly (implicit substring matrch).
- addBlack regex ^foo\..*\.cpu+ # ignore foo.<anything>.cpu.... (regex pattern match) 
- addAgg sum ^stats\.timers\.(app|proxy|static)[0-9]+\.requests\.(.*) stats.timers._sum_$1.requests.$2 10 20
- addAgg avg ^stats\.timers\.(app|proxy|static)[0-9]+\.requests\.(.*) stats.timers._avg_$1.requests.$2 5 10
- addRoute sendAllMatch carbon-default  127.0.0.1:2005 spool=true pickle=false
- addRoute sendAllMatch carbon-tagger sub==  127.0.0.1:2006  # all metrics with '=' in them are metrics2.0 format for tagger
- addRoute sendFirstMatch analytics regex=(Err/s|wait_time|logger)  graphite.prod:2003 prefix=prod. spool=true pickle=true  graphite.staging:2003 prefix=staging. spool=true pickle=true

carbonrelayng_instrumentation_graphite_addr: ""
carbonrelayng_instrumentation_graphite_interval: 1000 # ms

```

Dependencies
------------

- redouane.golang

License
-------

BSD