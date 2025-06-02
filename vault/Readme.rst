Secrets in ansible vaults
=========================
This directory contains ansible vaults that are not under version control.

To create a vault, first create the ``vault/{{ ansible_hostname }}.yml``
file manually and encrypt it with a password::

    ansible-vault encrypt vault/ansible_hostname.yml

Do modify the variables, use::

    ansible-vault encrypt vault/ansible_hostname.yml

The ``ansible_hostname.yml`` file is automatically picked up by the ``secrets``
task. If a file exists for a specified host, it must be decrypted for the
playbook to succeed. To use the vaults when running the playbook, use the ``-J``
option to be asked for the password::

    ansible-playbook -J -i production site.yml
