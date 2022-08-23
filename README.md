The purpose of this project is to remap the Wazuh data model to align as much as possible with ECS format

### Supported Log Sources ###

Linux:
- Auditd

Windows:
- Security event logs
- Sysmon event logs

### NOTE ###

It is recommended that you test these configs in a staging environment before deploying them to production.

Monitoring the elastic ingest nodes usage is highly recommended after these changes are made, there isn't any scripting done so it shouldn't be extremely resource greedy, but you should still monitor cpu usage

You will have to close or delete your existing indexes for these changes to occur, otherwise the change will occur during the next index cycle usually at midnight unless you're using new index management

- The field "full_log" has been a line to be removed from being indexed; if you wish to keep this field remove the line

### Implementation for wazuh-alerts-* only ###

backup the original wazuh-template.json file (/etc/filebeat/wazuh-template.json)

add the wazuh-template.json file from this repo 

from here you can either implement the new index template via dev tools or via filebeat

backup the original ingest pipeline (/usr/share/filebeat/module/wazuh/alerts/ingest/pipeline.json)

add the ingest pipeline file from this repo

again you can implement via filebeat or dev tools

### Implementation for alerts and archived data ###

The same process will be for archives however you will be targeting the following file for ingest template

ls /usr/share/filebeat/module/wazuh/archives/ingest/pipeline.json

either use filebeat to update the ingest template or dev tools

### To Do ###

Continue to refine exsisting fields
Add more advanced event information




