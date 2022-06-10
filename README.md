# Netbox Manage
---
> Roles To Deploy, Backup, Restore Netbox Instances

## Overview
---
`netbox-manage` ansible role will be utilized for managing Netbox in a Sandbox environment

## Usage
---
> Individual tasking will be handled through the use of `tags`

- `deploy`: Deploys NetBox to hosts utilizing docker-compose via Ansible
    - installs docker and components such as python modules
    - clones specified version of https://github.com/netbox-community/netbox-docker.git for deploy scripts
    - configurable in `roles/netbox-manage/defaults/main.yml`
    ```
    ansible-playbook playbook.yml -t deploy
    ```

- `backup`: Backs current NetBox DB to a local `.sql` file
    - saves sql file to `roles/netbox-manage/files/backups/` directory
    - `example-backup.sql` example backup file provided (names will follow timestamped pattern. ex: `netbox-2022-06-10T19:21:52.530422Z.sql`)
    ```
    ansible-playbook playbook.yml -t backup
    ```

- `restore`: Restores the latest NetBox DB backup unless specified otherwise.
    - Checks `roles/netbox-manage/files/backups` directory locally for the latest backup. (uses Ansible Find module to determine newest backup)
    - Can be customized to restore from alternate SQL file
    ```
    ansible-playbook playbook.yml -t restore

    # to specify which backup or sql file to use
    ansible-playbook playbook.yml -t restore \
    --extra-vars="latest_backup_path=roles/netbox-manage/files/backups/example-backup.sql"
    ```

- `upgrade` : Upgrades NetBox
> WIP : will add notes once complete