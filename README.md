Consul
======

Deploy [HashiCorp Consul](https://www.consul.io/) using ansible

This is meant as a minimal installation for small single-board computers.
While it does work for bigger installations, it may not have the features
and tuning options desired by such a deployment.

Requirements
------------

This role should work on every linux system with an arm32, arm64, x86, or 
amd64 CPU and systemd.

Role Variables
--------------

### Generic variables ###
| Name                 | Default Value  | Description                         |
| -------------------- | -------------- | ----------------------------------- |
| consul_masters       | n/a (required) | Join flags containing the consul servers |
| consul_version       | 1.5.3          | Version of consul to install        |
| consul_user          | consul         | User to run consul as               |
| consul_group         | consul         | Group to run consul as              |
| consul_config        | -              | Dictionary with config for consul, converted to json |
| consul_systemd_options | -            | Flags to send to consul in systemd  |
| consul_master_nodes  | 3              | Number of master nodes to expect    |
| consul_agent_mode    | client         | client for client mode, server for server mode |


### Paths ###
| Name                 | Default Value  | Description                         |
| -------------------- | -------------- | ----------------------------------- |
| consul_path          | /opt/consul    | Base path to consul installation    |
| consul_config_path   | /etc/consul    | Path to consul configuration        |
| consul_config_file   | "{{ consul_config_path }}/config.json" | Path to config file |
| consul_configd_path  | "{{ consul_config_path }}/consul.d" | Path to config dir |
| consul_data_path     | /var/consul    |  Path to data-storage location      |
| consul_log_path      | /var/log/consul | Path to log directory              |
| consul_log_file      | "{{ consul_log_path }}/consul.log" | Path to log file |
| consul_run_path      | /var/run/consul | Base path to run information       |
| consul_binary        | /usr/local/bin/consul | Path to consul binary (in $PATH) |


### Internal variables ###
| Name                | Default Value | Description                           |
| ------------------- | ------------- | ------------------------------------- |
| consul_platform     | -             | Platform part of consul package name  |
| consul_os           | -             | OS part of consul package name        |
| consul_release_name | -             | Name of consul release package        |
| consul_download_fn  | -             | Filename of consul release            |
| consul_download_url | -             | Download URL of consul package        |

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables
passed in as parameters) is always nice for users too:

    - hosts: sonsul-servers
      roles:
        - role: cvi.consul
          consul_agent_mode: server
          consul_masters: >-
            {% for host in groups['sonsul-servers'] | sort %}
            -retry-join {{ host }}
            {% endfor %}

    - hosts: sonsul-clients
      roles:
        - role: cvi.consul
          consul_masters: >-
            {% for host in groups['sonsul-servers'] | sort %}
            -retry-join {{ host }}
            {% endfor %}


License
-------

MIT
