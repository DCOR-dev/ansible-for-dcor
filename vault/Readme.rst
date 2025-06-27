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

A typical `ansible_hostname.yml` file contains the following keys::

    # dcor.example.com.yml
    DCOR_S3_ACCESS_KEY_ID: "laoisd8hfq09r83up93r8"
    DCOR_S3_SECRET_ACCESS_KEY: "dol198jlo98hedl9q834"
    CKAN_INI_SECRET_KEY: "AaLSODIJPl2893uep0w9fu"
    MAINTAINER_EMAIL: "dcor-maintainer@example.com"
