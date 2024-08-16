
# Role: AWX Installation on Kubernetes

This Ansible role installs and configures **AWX** on a Kubernetes cluster with a focus on K3s and persistent storage. The role automates the deployment of AWX, along with the setup of custom credentials, projects, inventories, and optional LDAP integration.

## Requirements

- **Ansible version:** 2.9 or higher
- **Kubernetes:** K3s or any Kubernetes cluster
- **Collections:** 
  - `community.general`
  - `kubernetes.core`
  - `community.kubernetes`

## Role Variables

### General Configuration
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `awx_namespace`                       | Namespace where AWX will be deployed                    | `awx`                            |
| `install_dir`                         | Location where files will be stored                     | `/tmp/awx`                       |
| `persistent_container_storage_dir`    | Directory where persistent storage will be mounted      | `/srv/containers`                |
| `awx_operator_version`                | AWX Operator version to install                         | latest available version         |

### AWX Administrator Configuration
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `awx_admin_user`                      | AWX admin username                                      | `admin`                          |
| `awx_admin_password`                  | AWX admin password                                      | `password`                       |
| `awx_db_user`                         | AWX database username                                   | `awx`                            |
| `awx_db_password`                     | AWX database password                                   | `password`                       |
| `awx_db_name`                         | AWX database name                                       | `awx`                            |

### Ingress Configuration
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `awx_use_cname`                       | Set to `true` to use custom CNAME for AWX Ingress        | `false`                         |
| `awx_cname`                           | Custom CNAME for AWX Ingress (if `awx_use_cname` is true)| `awx.local.lan                  |

### K3s Installation
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `k3s_install`                         | Set to `true` to install K3s                            | `false`                          |

### Namespace Deletion
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `delete_ns`                           | Set to `true` to delete the namespace and persistent volumes before redeploying | `false`  |

### Customization
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `set_logo`                            | Set to `true` to configure a custom logo in AWX          | `false`                         |
| `set_login_info`                      | Set to `true` to configure custom login information      | `false`                         |
| `set_ldap`                            | Set to `true` to configure LDAP settings                 | `false`                         |
| `logo_file_path`                      | Path to the custom logo file, only when logo_url not set | `{{ base_dir }}/resources/logo.png` |
| `logo_url`                            | URL to the custom logo file, will use this if set        | `{{ base_dir }}/resources/logo.png` |
| `custom_login_info_content`           | Custom login page HTML content                           | Custom HTML provided in defaults |

### AWX Credential Setup
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `set_credentials`                     | Set to `true` to configure credentials in AWX            | `true`                          |
| `credentials`                         | List of credentials to configure                         | See defaults/main.yml           |

### AWX Project Setup
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `set_projects`                        | Set to `true` to configure projects in AWX               | `true`                           |
| `projects`                            | List of projects to configure                            | See defaults/main.yml            |

### AWX Inventory Setup
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `set_inventories`                     | Set to `true` to configure inventories in AWX            | `true`                           |
| `inventories`                         | List of inventories to configure                         | See defaults/main.yml            |
| `hosts`                               | List of hosts to add to inventories                      | See defaults/main.yml            |

### LDAP Configuration (Optional)
| Variable                              | Description                                             | Default                          |
|---------------------------------------|---------------------------------------------------------|----------------------------------|
| `ldap_server_uri`                     | LDAP server URI                                          | `ldaps://your-ldap-server.com:636` |
| `ldap_bind_dn`                        | LDAP bind DN                                             | `"uid=awx,cn=sysaccounts,cn=etc,dc=yourdomain,dc=com"` |
| `ldap_bind_password`                  | LDAP bind password                                       | `"your_bind_password"`           |
| `ldap_start_tls`                      | Use TLS for LDAP                                         | `false`                          |
| `ldap_user_dn_template`               | Template for user DN in LDAP                             | `"uid=%(user)s,cn=users,cn=accounts,dc=yourdomain,dc=com"` |

## Usage

This role is designed to be flexible, supporting various configurations through variables. A typical playbook that uses this role might look like this:

```yaml
---
- hosts: localhost
  roles:
    - role: role_awx_install
      vars:
        awx_namespace: "awx"
        awx_admin_user: "admin"
        awx_admin_password: "MyStrongPassword"
        awx_db_password: "MyDbPassword"
        k3s_install: true
        set_logo: true
        logo_file_path: "/path/to/logo.png"
        set_projects: true
        projects:
          - name: "Project A"
            scm_type: "git"
            scm_url: "https://github.com/example/project.git"
```

You can customize the AWX instance, set up credentials, projects, inventories, and even configure LDAP if needed.

### Dependencies

Ensure that the required Ansible collections are installed:

```bash
ansible-galaxy collection install community.general kubernetes.core community.kubernetes
```

### Example Inventory

For testing, use the following inventory format:

```ini
localhost ansible_connection=local
```

### Author Information

This role was created by **Owen Vogelaar**.

## License

MIT License
