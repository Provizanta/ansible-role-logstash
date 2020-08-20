Ansible role: Logstash
=========

![Build & Deploy](https://github.com/Provizanta/ansible-role-logstash/workflows/molecule/badge.svg?branch=master)

Logstash installation and configuration role for ELK stack. Allows to install arbitrary plugins and introduce new pipeline configurations.

Requirements
------------

None

Role Variables
--------------

These variables are set in [defaults/main.yml](./defaults/main.yml):

    elk_version: 6

    elk_logstash_pipelines: {}   # dict, key represents pipeline name, value is the string content of the pipeline configuration file

    elk_logstash_plugins: []

    elk_logstash_service:
      enabled: true
      state: started

Non-defaulted variables that can be set:


Dependencies
------------

None

Example Playbook
----------------

    - name: Converge
      hosts: all
      roles:
        - role: logstash
          vars:
            elk_logstash_plugins:
              - logstash-filter-fingerprint
            elk_logstash_pipelines:
              production_pipeline: |
                input {
                  beats {
                    port => 32333
                  }
                }

                filter {
                  date {
                    match => '[ "timestamp", "yyyy-MM-dd HH:mm:ss,SSS" ]'
                  }
                }

                output {
                  elasticsearch {
                    hosts => ['localhost:9200']
                    index => '%{[@metadata][index_name]}-%{[@metadata][index_date]}'
                    action => 'update'
                    doc_as_upsert => true
                    document_id => '%{[@metadata][fingerprint]}'
                  }
                }

License
-------

MIT

Author Information
------------------

Tibor Csóka
