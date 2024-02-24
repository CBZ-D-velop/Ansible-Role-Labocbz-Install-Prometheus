# Ansible role: labocbz.install_prometheus

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: UNIX](https://img.shields.io/badge/Tech-UNIX-orange)
![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: Prometheus](https://img.shields.io/badge/Tech-Prometheus-orange)
![Tag: Standalone](https://img.shields.io/badge/Tech-Standalone-orange)
![Tag: Cluster](https://img.shields.io/badge/Tech-Cluster-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2TLS-orange)
![Tag: Federation](https://img.shields.io/badge/Tech-Federation-orange)

An Ansible role to install and configure Prometheus on your host.

The offered Ansible role streamlines the installation and configuration of Prometheus, a powerful monitoring solution. It simplifies the Prometheus setup process, encompassing SSL/TLS security, basic authentication, and the ability to establish federation for data scraping. This role not only facilitates the deployment of Prometheus instances but also empowers you to create a cohesive monitoring network.

Whether you're orchestrating monitoring across clusters or enabling data retrieval between instances, this role ensures straightforward configuration, guaranteeing seamless and secure data collection. This is especially beneficial for scenarios like extracting insights from various data centers or setting up instances to recover data from remote nodes. In essence, the role provides a comprehensive toolset to enhance your infrastructure monitoring effortlessly.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
install_prometheus__config_path: "/etc/prometheus"
install_prometheus__log_path: "/var/log/prometheus"
install_prometheus__port: "9090"
install_prometheus__host: "0.0.0.0"

install_prometheus__ssl_path: "{{ install_prometheus__config_path }}/ssl"
install_prometheus__scrape_interval: "10s"
install_prometheus__evaluation_interval: "10s"
install_prometheus__loglevel: "debug"

install_prometheus__ssl: true
install_prometheus__ssl_key: "{{ install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/my-prometheus-cluster.domain.tld.pem.key"
install_prometheus__ssl_crt: "{{ install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/my-prometheus-cluster.domain.tld.pem.crt"
install_prometheus__ssl_ca: "{{ install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/ca-chain.pem.crt"

install_prometheus__basic_auth: true
install_prometheus__basic_auth_login: "admin"
install_prometheus__basic_auth_password: "admin"
install_prometheus__basic_auth_password_hash: "$2a$10$0M5Kx/KYWNIExB1AfP0wDuMT6hGkkNOcxLtLRWV6nfSZWfonGb69W"

install_prometheus__clustername: "my-prometheus-cluster.domain.tld"
install_prometheus__instancename: "prom-1"
install_prometheus__retention_time: "1y"

install_prometheus__federate_collector_hosts:
  - "my.prom.instance.domain.tld:9090"

install_prometheus__federate_collector: false
install_prometheus__federate_collector_interval: "5s"

install_prometheus__jobs:
  - name: "My Prom instances"
    basic_auth: true
    basic_auth_login: "admin"
    basic_auth_password: "admin"
    ssl: true
    ca: "{{ install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/ca-chain.pem.crt"
    targets:
      - "my.prom-1.instance.domain.tld:{{ install_prometheus__port }}"
      - "my.prom-2.instance.domain.tld:{{ install_prometheus__port }}"
      - "my.prom-3.instance.domain.tld:{{ install_prometheus__port }}"

install_prometheus__group: "prometheus"
install_prometheus__user: "prometheus"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host__users:
  - login: "root"
    group: "prometheus"

inv_install_prometheus__config_path: "/etc/prometheus"
inv_install_prometheus__log_path: "/var/log/prometheus"
inv_install_prometheus__port: "9090"
inv_install_prometheus__host: "0.0.0.0"

inv_install_prometheus__ssl_path: "{{ inv_install_prometheus__config_path }}/ssl"
inv_install_prometheus__scrape_interval: "5s"
inv_install_prometheus__evaluation_interval: "1s"
inv_install_prometheus__loglevel: "debug"

inv_install_prometheus__ssl: true
inv_install_prometheus__ssl_key: "{{ inv_install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/my-prometheus-cluster.domain.tld.pem.key"
inv_install_prometheus__ssl_crt: "{{ inv_install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/my-prometheus-cluster.domain.tld.pem.crt"
inv_install_prometheus__ssl_ca: "{{ inv_install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/ca-chain.pem.crt"

inv_install_prometheus__basic_auth: true
inv_install_prometheus__basic_auth_login: "admin"
inv_install_prometheus__basic_auth_password: "admin"
inv_install_prometheus__basic_auth_password_hash: "$2a$10$0M5Kx/KYWNIExB1AfP0wDuMT6hGkkNOcxLtLRWV6nfSZWfonGb69W"

inv_install_prometheus__clustername: "my-prometheus-cluster.domain.tld"
inv_install_prometheus__instancename: "prom-1"
inv_install_prometheus__retention_time: "1y"

inv_install_prometheus__federate_collector_interval: "15s"

inv_install_prometheus__jobs:
  - name: "My Molecule instances (on SSL + Auth)"
    basic_auth: true
    basic_auth_login: "admin"
    basic_auth_password: "admin"
    ssl: true
    ca: "{{ inv_install_prometheus__ssl_path }}/my-prometheus-cluster.domain.tld/ca-chain.pem.crt"
    targets:
      - "molecule-local-instance-2-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-3-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-4-install-prometheus:{{ inv_install_prometheus__port }}"

  - name: "My Molecule instances (on SSL)"
    ssl: true
    targets:
      - "molecule-local-instance-2-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-3-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-4-install-prometheus:{{ inv_install_prometheus__port }}"

  - name: "My Molecule instances (on Auth)"
    basic_auth: true
    basic_auth_login: "admin"
    basic_auth_password: "admin"
    targets:
      - "molecule-local-instance-2-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-3-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-4-install-prometheus:{{ inv_install_prometheus__port }}"

  - name: "My Molecule instances"
    targets:
      - "molecule-local-instance-2-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-3-install-prometheus:{{ inv_install_prometheus__port }}"
      - "molecule-local-instance-4-install-prometheus:{{ inv_install_prometheus__port }}"

# From SCRAPER / Cluster / Federator
---

install_prometheus__instancename: "prom-1"

install_prometheus__federate_collector_hosts:
  - "molecule-local-instance-2-install-prometheus"
  - "molecule-local-instance-3-install-prometheus"
  - "molecule-local-instance-4-install-prometheus"

install_prometheus__federate_collector: true

```

```YAML
# From AWX / Tower
---
all vars from to put/from AWX / Tower
```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_prometheus"
  tags:
    - "labocbz.install_prometheus"
  vars:
    install_prometheus__log_path: "{{ inv_install_prometheus__log_path }}"
    install_prometheus__port: "{{ inv_install_prometheus__port }}"
    install_prometheus__host: "{{ inv_install_prometheus__host }}"
    install_prometheus__ssl_path: "{{ inv_install_prometheus__ssl_path }}"
    install_prometheus__scrape_interval: "{{ inv_install_prometheus__scrape_interval }}"
    install_prometheus__evaluation_interval: "{{ inv_install_prometheus__evaluation_interval }}"
    install_prometheus__loglevel: "{{ inv_install_prometheus__loglevel }}"
    install_prometheus__ssl: "{{ inv_install_prometheus__ssl }}"
    install_prometheus__ssl_key: "{{ inv_install_prometheus__ssl_key }}"
    install_prometheus__ssl_crt: "{{ inv_install_prometheus__ssl_crt }}"
    install_prometheus__ssl_ca: "{{ inv_install_prometheus__ssl_ca }}"
    install_prometheus__basic_auth: "{{ inv_install_prometheus__basic_auth }}"
    install_prometheus__basic_auth_login: "{{ inv_install_prometheus__basic_auth_login }}"
    install_prometheus__basic_auth_password: "{{ inv_install_prometheus__basic_auth_password }}"
    install_prometheus__basic_auth_password_hash: "{{ inv_install_prometheus__basic_auth_password_hash }}"
    install_prometheus__instancename: "{{ inv_install_prometheus__instancename }}"
    install_prometheus__clustername: "{{ inv_install_prometheus__clustername }}"
    install_prometheus__retention_time: "{{ inv_install_prometheus__retention_time }}"
    install_prometheus__federate_collector_interval: "{{ inv_install_prometheus__federate_collector_interval }}"
    install_prometheus__jobs: "{{ inv_install_prometheus__jobs }}"
  ansible.builtin.include_role:
    name: "labocbz.install_prometheus"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-05: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-08-05: Added Clustering and SSL/TLS

* Role handle SSL/TLS
* Role doest not handle password hashing for now
* Role chandle clustering Prometheus and local scraping

### 2023-08-05-b: Federation Job

* Role handler clustering AND external federation
* Added testing
* Added a validation task at the end of templating, so service dont restart if YAML is not good

### 2023-08-05-c: Scraps Jobs

* You can now define your jobs in the role
* Role handle job on SSL/TLS
* Job can use Basic Auth to

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

### 2023-11-17: Fix Federation, retention duration

* Federation cluster is Ok now (all datas scraped)
* Role handle now duration scraping
* Role write logs into a file now insted of sysout

### 2024-02-22: New CICD and fixes

* Added support for Ubuntu 22
* Added support for Debian 11/22
* Edited vars for linting (role name and __)
* Fix idempotency
* New CI, need work on tag and releases
* CI use now Sonarqube

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [GETTING STARTED PROMETHEUS](https://prometheus.io/docs/prometheus/latest/getting_started/)
* [How To Install Prometheus on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-prometheus-on-ubuntu-16-04)
* [Password file generator for Prometheus](https://o11y.tools/pwgen/)
