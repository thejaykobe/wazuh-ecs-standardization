The purpose of this project is to remap the Wazuh data model to align as much as possible with ECS format based on the csv provided by elastic found here: https://github.com/elastic/ecs/blob/main/generated/csv/fields.csv

### Supported Log Sources ###

Linux:
- Auditd
- Snoopy auditing for linux

Windows:
- Security event logs
- Sysmon event logs

### Example ###

![alt text](https://github.com/thejaykobe/wazuh-ecs-standardization/blob/main/Example-sysmon-event%20id%201.png?raw=true)

### NOTE ###

- Auditd was enabled on a test host and the audit.rules are enabled from Neo23x0 -> https://github.com/Neo23x0/auditd

- Sysmon was enabled with the sysmon.xml file from olafhartong using the "verbose-sysmonconfig-excludes-only" -> https://github.com/olafhartong/sysmon-modular

- Snoopy was enabled with the following config in '/etc/snoopy.ini'

'message_format = "[%{datetime}] %{hostname} snoopy: uid=%{uid} pid=%{pid} currentdir=%{cwd} suser=%{login} duser=%{tty_username} domain=%{domain} ppid=%{ppid} cmdline=%{cmdline}"'

Snoopy can be found here ---> https://github.com/a2o/snoopy

---

It is recommended that you test these configs in a staging environment before deploying them to production.

Monitoring the elastic ingest nodes usage is highly recommended after these changes are made, there isn't any scripting done so it shouldn't be extremely resource greedy, but you should still monitor cpu usage

You will have to close or delete your existing indexes for these changes to occur, otherwise the change will occur during the next index cycle usually at midnight unless you're using new index management

- The field "full_log" has been a line to be removed from being indexed; if you wish to keep this field remove the line

### Easy Deployment ###

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

### Advanced Deployment ###

If you want to specificy dedicated pipelines per log type there is the advanced folder

You will basically follow the steps from above, and you'll also need to add the following pipelines in dev tools or api like so:

- PUT _ingest/pipeline/wazuh-auditd
- PUT _ingest/pipeline/wazuh-windows
- PUT _ingest/pipeline/wazuh-wazuh

### To Do ###

Continue to refine exsisting fields
Add more advanced event information

### Additions not "ECS" ###

Auditd:
- added support for user id to translate into source.user.name

Sysmon:
- Added a source or destination machine accounts value in the tags field