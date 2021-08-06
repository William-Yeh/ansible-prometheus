
william-yeh.prometheus for Ansible Galaxy
============

[![Circle CI](https://circleci.com/gh/William-Yeh/ansible-prometheus.svg?style=shield)](https://circleci.com/gh/William-Yeh/ansible-prometheus) [![Build Status](https://travis-ci.org/William-Yeh/ansible-prometheus.svg?branch=master)](https://travis-ci.org/William-Yeh/ansible-prometheus)



## Summary

Role name in Ansible Galaxy: **[william-yeh.prometheus](https://galaxy.ansible.com/William-Yeh/prometheus)**

This Ansible role has the following features for [Prometheus](http://prometheus.io/):

 - Install specific versions of [Prometheus server](https://github.com/prometheus/prometheus), [Node exporter](https://github.com/prometheus/node_exporter), [Alertmanager](https://github.com/prometheus/alertmanager).
 - Handlers for restart/reload/stop events;
 - Bare bone configuration (*real* configuration should be left to user's template files; see **Usage** section below).


To keep this role simple, this role only installs 3 components: Prometheus server, Node exporter, and Alertmanager. Use the following roles if you want to install other Prometheus exporters:

- Consul: **[william-yeh.consul_exporter](https://galaxy.ansible.com/william-yeh/consul_exporter/)**
- Elasticsearch: **[william-yeh.es_cluster_exporter](https://galaxy.ansible.com/william-yeh/es_cluster_exporter/)**
- MongoDB: **[williamyeh.mongodb_exporter](https://galaxy.ansible.com/williamyeh/mongodb_exporter/)**


Supports for Ubuntu 12.04 (Precise) and CentOS 6 have been ended since Nov 2018.



## Role Variables


### Mandatory variables

The components to be installed:

```yaml
# Supported components:
#
#   [Server components]
#     - "prometheus"
#     - "alertmanager"
#
#   [Exporter components]
#     - "node_exporter"
#
prometheus_components
```



### Optional variables: general settings


User-configurable defaults:

```yaml
# user and group
prometheus_user:   prometheus
prometheus_group:  prometheus


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
gosu_version:  "1.11"
```


### Optional variables: systemd or not


If the Linux distributions are equipped with systemd, this role will use this mechanism accordingly. You can disable this (i.e., use traditional SysV-style init script) by defining the following variable(s) to `false`:

```yaml
prometheus_use_systemd
```



### Optional variables: Prometheus server

User-configurable defaults:

```yaml
# which version?
prometheus_version:  2.28.1



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


User-installable rule files (see [doc](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/) for details):


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
prometheus_alertmanager_hostport
```


Additional command-line arguments, if any (use `prometheus --help` to see the full list of arguments):

```yaml
prometheus_opts
```


### Optional variables: Node exporter


User-configurable defaults:

```yaml
# which version?
prometheus_node_exporter_version:  1.2.0
```

Additional command-line arguments, if any (use `node_exporter --help` to see the full list of arguments):

```yaml
prometheus_node_exporter_opts
```


### Optional variables: Alertmanager


User-configurable defaults:

```yaml
# which version?
prometheus_alertmanager_version:  0.22.2

# directory for runtime database (currently for `silences.json`)
prometheus_alertmanager_db_path: /var/lib/alertmanager
```

User-installable alertmanager conf file (see [doc](http://prometheus.io/docs/alerting/alertmanager/) for details):

```yaml
# main conf template relative to `playbook_dir`;
# to be installed to "{{ prometheus_config_path }}/alertmanager.yml"
prometheus_alertmanager_conf
```

Additional command-line arguments, if any (use `alertmanager --help` to see the full list of arguments):

```yaml
prometheus_alertmanager_opts
```


### Optional: building from source tree

(Credit: [Robbie Trencheny](https://github.com/robbiet480))

For aforementioned `prometheus_components`, you can optionally download/compile from the *master* branch of [Prometheus repositories](https://github.com/prometheus) by setting the respective version to `git`.

It will install a temporary Golang compiler in the `prometheus_workdir` directory (defined in `defaults/main.yml`).

For example, get the latest code for all components by assigning all `*_version` variables to `git`:

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

- `reload node_exporter` (actually, the same as `restart`)

- `stop node_exporter`


Alertmanager:

- `restart alertmanager`

- `reload alertmanager`

- `stop alertmanager`



## Usage


### Step 1: add role

Add role name `william-yeh.prometheus` to your playbook file.


### Step 2: add variables

Set vars in your playbook file, if necessary.

Simple example:

```yaml
---
# file: simple-playbook.yml

- hosts: all
  become: True
  roles:
    - william-yeh.prometheus

  vars:
    prometheus_components: [ "prometheus", "alertmanager" ]

    prometheus_alertmanager_hostport: "localhost:9093"
```


### Step 3: copy user's config files, if necessary


More practical example:

```yaml
---
# file: complex-playbook.yml

- hosts: all
  become: True
  roles:
    - william-yeh.prometheus

  vars:
    prometheus_components:
      - prometheus
      - node_exporter
      - alertmanager

    prometheus_rule_files:
      this_is_rule_1_InstanceDown:
        src:  some/path/basic.rules
        dest: basic.rules

    prometheus_alertmanager_conf: some/path/alertmanager.yml.j2
```


### Step 4: browse the default Prometheus pages

Open the page in your browser:

- Prometheus - `http://HOST:9090` or `http://HOST:9090/consoles/node.html`

- Alertmanager - `http://HOST:9093`


## Dependencies

None.


## Contributors

- [William Yeh](https://github.com/William-Yeh)
- [Robbie Trencheny](https://github.com/robbiet480) - contribute an early version of building binaries from Go source code.
- [Travis Truman](https://github.com/trumant) - contribute an early version of consul_exporter installer; now moved to [William-Yeh.consul_exporter](https://github.com/William-Yeh/ansible-consul-exporter).
- [Musee Ullah](https://github.com/lae)
- [Alexandre Rodrigues](https://github.com/LexVar)

## License

MIT License. See the [LICENSE file](LICENSE) for details.
