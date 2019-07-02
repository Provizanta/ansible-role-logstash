Ansible role: Logstash
=========

Logstash installation and configuration role for ELK stack. Allows to install arbitrary plugins and introduce new pipeline configurations.

Requirements
------------

None

Role Variables
--------------

These defaults are set in defaults/main.yml:

    version: 6

    pipelines: {}   # dict, key represents pipeline name, value is the string content of the pipeline configuration file

    plugins: []

    service:
      enabled: true
      state: started

Non-defaulted variables that can be set:


Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: logstash
          vars:
            plugins:
              - logstash-filter-fingerprint
            pipelines:
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
