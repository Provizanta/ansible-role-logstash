Logstash
=========

Logstash connector proxy for Elasticsearch cluster.

Requirements
------------

None

Role Variables
--------------

    pipeline_configuration: <list of pipeline configurations>
      - name: <pipeline name - retains only alphanumeric chars and underscore>
        input:
          type: <list of inputs (dictionaries)>
        filter:
          __if: <an 'if' clause - uses a string predicate (at toplevel or element level)> 
          type: <list filters (dictionaries)>
        output:
          type:
            - __if: <an 'if' clause - block level predicate string> 
              <output>:
                <setting> 

Dependencies
------------

common/elk/base

Example Playbook
----------------

    - hosts: servers
      roles:
        - role: common/elk/logstash
          vars:
            pipeline_configuration:
              - name: Random index pipeline 1
                input:
                  type:
                    - beats:
                        port: 8854
                filter:
                  __if: '"random-index-data" in [tags]'
                  type:
                    - mutate:
                        remove_field: "{{ removable_filebeat_fields }}"
                    - json:
                        source: message
                output:
                  type:
                    - __if: '"random-index-data" in [tags]'
                      elasticsearch:
                        hosts: 127.0.0.1:9200
                        manage_template: false
                        index: "random-index-%{+YYYY.MM.dd}"
                        document_type: _doc
      vars:
        removable_filebeat_fields: ["input", "type", "beat", "input_type", "offset", "source", "fields", "prospector"]

License
-------

MIT

Author Information
------------------

Tibor Csoka
