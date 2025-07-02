Public keys for instance backup encryption
==========================================
DCOR can create encrypted database backups and store them on S3.
This allows you to only worry about backing up your S3 object storage.
For every file named according to the pattern ``key_KEY-SIGNATURE.pub``,
(e.g. `key_2B4B066FFE3D288E.pub`), this ansible playbook will
create a cron job that creates an encrypted instance backup
and uploads it to S3 using the command
``dcor encrypted-instance-backup --key-id KEY-SIGNATURE``.

If you have to recover from backup, you have to decrypt the
backup first::

    gpg --output backup.tar.bz2 --decrypt backup_2B4B066FFE3D288E_dcor-control.tar.bz2.gpg

Then, move it to ``backups/host-{{ FULLY_QUALIFIED_DOMAIN_NAME }}/restore_instance.tar.bz2``
and run::

    ansible-playbook -i YOURINVENTORY backup-restore.yml
