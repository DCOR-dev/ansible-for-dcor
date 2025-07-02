Ansible for DCOR
================

These are ansible playbooks for setting up and upgrading DCOR on a
vanilla Ubuntu 24.04 machine.

To get started on your control node, install ansible::

    pipx install ansible-core ansible
    # https://github.com/hifis-net/ansible-collection-toolkit
    ansible-galaxy collection install hifis.toolkit
    ansible-galaxy collection install community.general
    # https://docs.ansible.com/ansible/latest/collections/community/postgresql/index.html
    ansible-galaxy collection install community.postgresql
    # mounting cache partition automatically
    ansible-galaxy collection install ansible.posix

The following roles are available:

- ``backup``: Create and restore backups (see below)
- ``ckan``: Install CKAN with all its dependencies (including postgresql and Solr)
- ``common``: General administrative tasks like unattended-upgrades
- ``dcor``: Install DCOR on top of an existing CKAN installation
- ``maintenance``: put a DCOR instance into maintenance mode
- ``secrets``: Set up internal secrets for DCOR

Notes:

- The ``site.yml`` playbook does not touch the redis or postgresql databases.
  Only access credentials are modified.


Backups
-------
The ``backup-create.yml`` and ``backup-restore.yml`` playbooks can be used to
create unencrypted backups of the CKAN database and the CKAN storage directory
(containing custom uploads such as user and organization images).

To create a backup, run::

    ansible-playbook -i staging backup-create.yml

This will create a compressed backup in the corresponding ``backups/host-*`` directory.
For restoring a backup, you have to rename it to ``restore_instance.tar.bz2`` in the
corresponding host-specific directory. Note that if the ``CKAN_INI_STORAGE_PATH``
variable is different on the restore host, you will have to either manually
move the files after restoring or modify the backup file in-place.
To restore a backup, run::

    ansible-playbook -i staging backup-restore.yml

Only hosts for which the ``restore_instance.tar.bz2`` files exist will be restored
from backup.
