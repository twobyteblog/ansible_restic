## Ansible Role: Restic

Ansible role which installs and configures Restic for file-level backups.

## Prerequisites

- Debian 10+
- Restic Repository Credentials

## Actions Performed

1. Installs [Restic](https://restic.net/).
2. Generates and installs a script for maintaining the Restic backup process.
3. Generates a secret's file used to store the backup repository credentials.
4. Installs a crontab entry to automate the backup process.

## Setup Instructions

### Clone Repository

Clone the project into the roles directory of your Ansible Controller.

```
git clone https://github.com/inundationca/ansible_restic.git
```

### Variables

Within your host or group variables, specify the path(s) you wish to have backed up.

```yaml
# Single path.
restic_backup_paths: "/var/www/html"

# Multiple paths.
restic_backup_paths: "/var/www/html /etc/apache2"
```

Lastly, specify the environment variables to allow Restic to access the repository. These variables can be determined via [Restic's documentation](https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html).

```yaml
restic_env_vars:
  RESTIC_REPOSITORY: repo01
  RESTIC_PASSWORD: password
  ...
```

For example, if backing up to Microsoft Azure:

```yaml
restic_env_vars:
  RESTIC_REPOSITORY: repo01
  RESTIC_PASSWORD: password
  AZURE_ACCOUNT_NAME: name
  AZURE_ACCOUNT_KEY: key
```

### Default Values

The following variables can be specified within your host or group variables to overwrite defaults set within the role.

```yaml
# Directory where Restic configuration is stored.
restic_config: "/opt/restic-backup"

# Default retention values for backups.
restic_retention_days: 14
restic_retention_weeks: 4
restic_retention_months: 12
restic_retention_years: 3

# Default logging location.
restic_logfile: "/var/log/restic-backup.log"
```

### Playbook

Create a playbook which runs this role. This role requires ```become``` privileges.

```yaml
- hosts: restic
  roles:
    - role: ansible_restic
      become: yes
```