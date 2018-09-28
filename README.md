Role Name
=========

Logstash connector role.

Requirements
------------

None

Role Variables
--------------

elastic:
  host: "127.0.0.1"
  port: "9200"
  index: "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"

logstash:
  config_path: "/etc/logstash/conf.d/"

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
    - role: common/elk/logstash
      vars:
        elastic:
          index: "index name"
        logstash:
          tcp:
            port: "5098"

License
-------

MIT

Author Information
------------------

Tibor Csoka, csokat@gmail.com
