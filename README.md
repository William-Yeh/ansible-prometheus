
williamyeh.prometheus for Ansible Galaxy
============

[![Circle CI](https://circleci.com/gh/William-Yeh/ansible-prometheus.svg?style=shield)](https://circleci.com/gh/William-Yeh/ansible-prometheus)



## Summary

Role name in Ansible Galaxy: **[williamyeh.prometheus](https://galaxy.ansible.com/list#/roles/4357)**

This Ansible role has the following features for [Prometheus](http://prometheus.io/):

 - Install specific versions of Prometheus server, node_exporter, and alertmanager.
 - Handlers for restart/reload/stop events;
 - Bare bone configuration (*real* configuration should be left to user's template files; see **Usage** section below).




## Role Variables

### Versioning note
For all 3 of the possible `prometheus_components` you can set the respective version to `git` to download the latest code and compile from the Prometheus repositories.
There are no external requirements needed to compile from source, as Prometheus provides them all via the install scripts.

For example, you can set all three variables to `git` to get the latest code for each component.

```yaml
prometheus_version: git
prometheus_node_exporter_version: git
prometheus_alertmanager_version: git
```

### Mandatory variables

None.



### Optional variables: genaral settings


User-configurable defaults:

```yaml
# user and group
prometheus_user:   prometheus
prometheus_group:  prometheus


# which components to install?
# possible values:
#  - "prometheus"
#  - "node_exporter"
#  - "alertmanager"
prometheus_components: [ "prometheus", "node_exporter" ]


# directory for executable files
prometheus_install_path:   /opt/prometheus

# directory for configuration files
prometheus_config_path:    /etc/prometheus

# directory for logs
prometheus_log_path:       /var/log/prometheus

# directory for PID files
prometheus_pid_path:       /var/run/prometheus



# directory for temporary files
prometheus_download_path:  /tmp


# version of helper utility "gosu"
gosu_version:  1.4
```



### Optional variables: Prometheus server

User-configurable defaults:

```yaml
# which version?
prometheus_version:  0.15.1



# directory for rule files
prometheus_rule_path:  {{ prometheus_config_path }}/rules

# directory for file_sd files
prometheus_file_sd_config_path:  {{ prometheus_config_path }}/tgroups

# directory for runtime database
prometheus_db_path:   /var/lib/prometheus
```






User-installable configuration file (see [doc](http://prometheus.io/docs/operating/configuration/) for details):


```yaml
# main conf template relative to `playbook_dir`;
# to be installed to "{{ prometheus_config_path }}/prometheus.yml"
prometheus_conf_main
```


User-installable rule files (see [doc](http://prometheus.io/docs/alerting/rules/) for details):


```yaml
# rule files to be installed to "{{ prometheus_rule_path }}" directory;
# dict fields:
#   - key: memo for this rule
#   - value:
#     - src:  file relative to `playbook_dir`
#     - dest: target file relative to `{{ prometheus_rule_path }}`
prometheus_rule_files
```


Alertmanager to be triggered:

```yaml
prometheus_alertmanager_url
```


### Optional variables: Node exporter


User-configurable defaults:

```yaml
# which version?
prometheus_node_exporter_version:  0.11.0
```



### Optional variables: Alert manager


User-configurable defaults:

```yaml
# which version?
prometheus_alertmanager_version:  0.0.3

# directory for runtime database
prometheus_db_path:   /var/lib/prometheus
```



User-installable alertmanager conf file (see [doc](http://prometheus.io/docs/alerting/alertmanager/) for details):


```yaml
# main conf template relative to `playbook_dir`;
# to be installed to "{{ prometheus_config_path }}/alertmanager.conf"
prometheus_alertmanager_conf

```


## Handlers

Prometheus server:

- `restart prometheus`

- `reload prometheus`

- `stop prometheus`

Alert manager:

- `restart alertmanager`

- `reload alertmanager`

- `stop alertmanager`



## Usage


### Step 1: add role

Add role name `williamyeh.prometheus` to your playbook file.


### Step 2: add variables

Set vars in your playbook file, if necessary.

Simple example:

```yaml
---
# file: simple-playbook.yml

- hosts: all
  sudo: True
  roles:
    - williamyeh.prometheus

  vars:
    prometheus_components: [ "prometheus", "alertmanager" ]

    prometheus_alertmanager_url: "http://localhost:9093/"
```


### Step 3: copy user's config files, if necessary


More practical example:

```yaml
---
# file: complex-playbook.yml

- hosts: all
  sudo: True
  roles:
    - williamyeh.prometheus

  vars:
    prometheus_rule_files:
      this_is_rule_1_InstanceDown:
        src:  some/path/basic.rules
        dest: basic.rules

    prometheus_alertmanager_conf: some/path/alertmanager.conf.j2
```


### Step 4: browse the default Prometheus page

Open “`http://`HOST`:9090`” in your browser.


## Dependencies

None.


## License

MIT License. See the [LICENSE file](LICENSE) for details.
