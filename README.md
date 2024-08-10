Role AWX Install
=========

This Ansible role is designed to install and configure AWX on a Kubernetes cluster, with a focus on using K3s and enabling persistent storage. It includes options for backup and restore, automatic deployment, and customization based on your Kubernetes environment.

Requirements
------------
- Ansible 2.9 or later
- A running Kubernetes cluster (e.g., K3s, Minikube)
- Required Ansible Collections:
  - community.general
  - kubernetes.core
  - community.kubernetes

Role Variables
------------
Below are the default variables that can be overridden as needed:

General Variables
---------
- awx_namespace: The namespace where AWX will be deployed. Default is "awx".
- bastion_install_dir: The directory on the bastion host where installation-related files will be stored. Default is "/tmp/awx".
- persistent_container_storage_dir: The directory where persistent storage locations are mounted. Default is "/srv/containers".

AWX Operator
---------
- awx_operator_version: The version of the AWX Operator to be installed. If not defined, the latest version is used.

K3S Installation
---------
- k3s_install: Set to true to install K3s as part of this role. Default is false.

Backup and Restore Options
---------
- backup_restore_data: Set to true to enable backup and restore functionality for migrating from a non-persistent to a persistent AWX deployment. Default is false.

Namespace Deletion
---------
- delete_ns: Set to true to delete the namespace and associated persistent volumes before redeploying AWX. Default is false.

Dependencies
---------
This role depends on the following Ansible collections:
- community.general
- kubernetes.core
- community.kubernetes

Ensure these collections are installed before running the role.

Example Playbook
---------
```yaml
---
- hosts: awx
  become: true
  vars:
    awx_namespace: "awx"
    bastion_install_dir: "/tmp/awx"
    persistent_container_storage_dir: "/srv/containers"
    backup_restore_data: false
    delete_ns: false
    k3s_install: true
  roles:
    - role_awx_install
```

License
---------
MIT

Author Information
---------
This role was created by Owen Vogelaar.