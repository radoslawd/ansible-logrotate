Logrotate
=========

A role that installs and configures Logrotate, as well as allowing other roles to configure their specific logrotate settings.

Requirements
------------

At the moment this has only been tested with RHEL 7 servers.

Role Variables
--------------

```
logrotate_rotation_interval: "weekly"
logrotate_rotation_amount: 4
logrotate_applications:
  - name: syslog
    path:
    - "/var/log/cron"
    - "/var/log/maillog"
    - "/var/log/messages"
    - "/var/log/secure"
    - "/var/log/spooler"
    options:
    - "missingok"
    - "sharedscripts"
    scripts:
    - type: postrotate
      script: "/bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true"
```

As a role dependency
--------------------

This role can be started as a dependency, so logrotate configuration can be stored with the application. Simply call it as a dependency, using the `as_dependency` tag and giving your `logrotate_applications` vars:

```
dependencies:
  - role: jamesbelchamber.logrotate
    tags:
    - as_dependency
    logrotate_applications:
      - name: my_cool_app
        path:
        - "/var/log/my_cool_app"
        options:
        - "missingok"
        - "delaycompress"
        - "notifempty"
        scripts:
        - type: postrotate
          script: "/sbin/service my_cool_app restart || true"
```

License
-------

GPLv3

Author Information
------------------

This role was created in 2016 by [James Belchamber](http://james.belchamber.com/)
