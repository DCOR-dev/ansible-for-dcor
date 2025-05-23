DCOR Ansible Control
====================

These are ansible playbooks for setting up and upgrading DCOR on a
vanilla Ubuntu 24.04 machine.

To get started on your control node, install ansible::

    pipx install ansible-core ansible
    # https://github.com/hifis-net/ansible-collection-toolkit
    ansible-galaxy collection install hifis.toolkit
    ansible-galaxy collection install community.general
    # https://docs.ansible.com/ansible/latest/collections/community/postgresql/index.html
    ansible-galaxy collection install community.postgresql

The following roles are available:

- common: General administrative tasks like unattended-upgrades
- ckan: Install CKAN with all its dependencies (including postgresql and Solr)
