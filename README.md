# Ansible Role: Restic

Ansible role which installs and configures Restic for file-level backups. This role:

1. Installs [Restic](https://restic.net/).
2. Generates and installs a script for maintaining the Restic backup process.
3. Generates a secret's file used to store the backup repository credentials.
4. Installs a crontab entry to automate the backup process.

The following packages will be installed when this role executes:

- restic

# Requirements

This role requires root access to the host system. Specify it globally, or add it to your playbook (example below).

```
- hosts: restic
  roles:
    - role: ansible_restic
      become: yes
```

# Installation

To use this role, clone the respository into the directory containing your other Ansible roles.

```
git clone https://github.com/inundationca/ansible_restic.git
```

Add it to a new or existing playbook.

```
- hosts: webapps
  roles:
    - role: ansible_restic
      become: yes
```

Within your host or group variables, specify the path(s) you wish to have backed up.

```
# Single path.
restic_backup_paths: "/var/www/html"

# Multiple paths.
restic_backup_paths: "/var/www/html /etc/apache2"
```

Lastly, specify the environment variables to allow Restic to access the repository. These variables can be determined via [Restic's documentation](https://restic.readthedocs.io/en/latest/030_preparing_a_new_repo.html).

```
restic_env_vars:
  RESTIC_REPOSITORY: repo01
  RESTIC_PASSWORD: password
  ...
```

For example, if backing up to Microsoft Azure:

```
restic_env_vars:
  RESTIC_REPOSITORY: repo01
  RESTIC_PASSWORD: password
  AZURE_ACCOUNT_NAME: name
  AZURE_ACCOUNT_KEY: key
```

# Default Values

The following variables can be specified within your host or group variables to overwrite defaults set within the role.

```
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

