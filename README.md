
williamyeh.prometheus for Ansible Galaxy
============

[![Circle CI](https://circleci.com/gh/William-Yeh/ansible-prometheus.svg?style=shield)](https://circleci.com/gh/William-Yeh/ansible-prometheus) [![Build Status](https://travis-ci.org/William-Yeh/ansible-prometheus.svg?branch=master)](https://travis-ci.org/William-Yeh/ansible-prometheus)



## Summary

Role name in Ansible Galaxy: **[williamyeh.prometheus](https://galaxy.ansible.com/list#/roles/4357)**

This Ansible role has the following features for [Prometheus](http://prometheus.io/):

 - Install specific versions of [Prometheus server](https://github.com/prometheus/prometheus), [Node exporter](https://github.com/prometheus/node_exporter), and [Alertmanager](https://github.com/prometheus/alertmanager).
 - Handlers for restart/reload/stop events;
 - Bare bone configuration (*real* configuration should be left to user's template files; see **Usage** section below).




## Role Variables


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

# Web listening address
prometheus_web_listen_addr: ":9090"
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



### Optional variables: Alertmanager


User-configurable defaults:

```yaml
# which version?
prometheus_alertmanager_version:  0.0.4

# directory for runtime database
prometheus_alertmanager_db_path: /var/lib/alertmanager
```



User-installable alertmanager conf file (see [doc](http://prometheus.io/docs/alerting/alertmanager/) for details):


```yaml
# main conf template relative to `playbook_dir`;
# to be installed to "{{ prometheus_config_path }}/alertmanager.conf"
prometheus_alertmanager_conf
```


### Optional: building from source

For aforementioned `prometheus_components`, you can optionally download/compile from the *master* branch of Prometheus repositories by setting the respective version to `git`.

There are no external requirements needed to compile from source, as Prometheus provides them all via the install scripts. Git, Mercurial, GZip, Curl, and Wget will be installed from your package manager (yum or apt). If you already have all dependencies, nothing will be installed.

For example, you can set all three variables to `git` to get the latest code for each component:

```yaml
prometheus_version: git
prometheus_node_exporter_version: git
prometheus_alertmanager_version: git
```

If you'd like to force rebuild each time, enable the following variable (default is `false`):

```yaml
prometheus_rebuild: true
```


## Handlers

Prometheus server:

- `restart prometheus`

- `reload prometheus`

- `stop prometheus`


Node exporter:

- `restart node_exporter`

- `reload node_exporter`

- `stop node_exporter`


Alertmanager:

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


### Step 4: browse the default Prometheus pages

Open the page in your browser:

- Prometheus - `http://`HOST`:9090` or `http://`HOST`:9090/consoles/node.html`

- Alertmanager - `http://`HOST`:9093`


## Dependencies

None.


## License

MIT License. See the [LICENSE file](LICENSE) for details.
