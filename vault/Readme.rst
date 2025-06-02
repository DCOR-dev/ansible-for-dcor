Secrets in ansible vaults
=========================
This directory contains (encrypted) variables that are not under version control.
You can just put plain yml files here or encrypt them with ansible vault.

To create a vault, first create the ``vault/{{ ansible_hostname }}.yml``
file manually. Then encrypt it with a password::

    ansible-vault encrypt vault/ansible_hostname.yml

To modify the variables, use::

    ansible-vault encrypt vault/ansible_hostname.yml

The ``ansible_hostname.yml`` file is automatically picked up by the ``secrets``
task. If a file exists for a specified host and is a vault, it must be decrypted
for the playbook to run. To use the vaults when running the playbook, use the ``-J``
option and you will be asked for the password::

    ansible-playbook -J -i production site.yml
