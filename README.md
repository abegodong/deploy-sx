deploy-sx
=========

This role will deploy a [Skylable SX cluster](http://www.skylable.com/products/sx/) on the nodes you specify.
After this role runs you will have an SX cluster up and running with all the nodes joined already.

Requirements
------------

This role can be used only for deploying on Linux systems running CentOS/Fedora/Debian/Ubuntu.

Role Variables
--------------

| Global Variable          | Default Value                    | Explanation                                        |
|--------------------------|----------------------------------|----------------------------------------------------|
| `sx_cluster_name`        | -                                | name of SX cluster (required)                      |
| `sx_nodes_group`         | -                                | name of the host group to deploy SX on             |
| `cert_key_size`          | -                                | size of SSL certificate key (2048 recommended)     |
| `cert_country_code`      | -                                | country code for SSL certificate                   |
| `cert_locality_name`     | -                                | locality name for SSL certificate                  |
| `cert_organization_name` | -                                | org name for SSL certificate                       |
| `cert_validity`          | -                                | validity in days for SSL certificate               |
| `sx_use_ssl`             | `true`                           | require SSL for server communication (recommended) |
| `sx_http_port`           | `80`                             | port to use for HTTP (if `sx_use_ssl` is `false`)  |
| `sx_https_port`          | `443`                            | port to use for HTTPS (if `sx_use_ssl` is `true`)  |
| `sx_data_dir`            | `/var/lib/sxserver/storage`      | where to store the data on each node               |
| `sx_log_file`            | `/var/log/sxserver/sxserver.log` | path to logfile                                    |
| `sx_server_user`         | `nobody`                         | server will run as this user                       |
| `sx_children_num`        | `32`                             | number of concurrent requests supported            |

| Per host variable     | Default value                  | Explanation                                                      |
|-----------------------|--------------------------------|------------------------------------------------------------------|
| `sx_node_size`        | -                              | maximum size of each node (accepted suffixes: `M`,`G`,`T`)       |
| `sx_node_ip`          | `ansible_default_ipv4.address` | public IP address of the node                                    |
| `sx_node_internal_ip` | `sx_node_ip`                   | private IP address of the node (used for internal communication) |

Dependencies
------------

None

Example Playbook
----------------

Variables which don't have a default value have to be set: per role for variables affecting the entire cluster, and per host
for variables affecting just one host.

    # example inventory:
    # [sxnodes]
    # sx-01 ansible_ssh_host=192.168.59.106 sx_node_size=1G sx_node_ip=192.168.59.106 sx_node_internal_ip=10.0.0.1
    # sx-02 ansible_ssh_host=192.168.59.107 sx_node_size=1G sx_node_ip=192.168.59.107 sx_node_internal_ip=10.0.0.2
    # sx_node_ip defaults to ansible_default_ipv4.address
    # sx_node_internal_ip defaults to empty -> use sx_node_ip
    ---
    - hosts: sxnodes
      remote_user: root
      roles:
        - role: skylable.deploy-sx
          sx_cluster_name: testcluster
          sx_nodes_group: sxnodes
          cert_key_size: 2048
          cert_country_code: UK
          cert_locality_name: London
          cert_organization_name: Skylable
          cert_validity: 3650
          sx_use_ssl: false
      
License
-------

CC-BY

Author Information
------------------

[Contact us](http://www.skylable.com/company/#form)
