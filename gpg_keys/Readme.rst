Public keys for instance backup encryption
==========================================
DCOR can create encrypted database backups and store them on S3.
This allows you to only worry about backing up your S3 object storage.
For every file named according to the pattern ``key_KEY-SIGNATURE.pub``,
(e.g. ``key_2B4B066FFE3D288E.pub``), this ansible playbook will
create a cron job that performs an encrypted instance backup
and uploads it to S3 using the command ``dcor encrypted-instance-backup --key-id KEY-SIGNATURE``.

If you have to recover from backup, you have to decrypt the
backup first::

    gpg --output backup.tar.bz2 --decrypt backup_2B4B066FFE3D288E_dcor-control.tar.bz2.gpg


Then, move it to ``backups/host-{{ FULLY_QUALIFIED_DOMAIN_NAME }}/restore_instance.tar.bz2``
and run::

    ansible-playbook -i YOURINVENTORY backup-restore.yml


Note that the ``key_*.pub`` files are not under version control. This makes
sure that cron jobs are created only for the keys explicitly put in this
directory.


Adding keys to the playbook
===========================
If you have an .asc file, convert it to a .gpg file with::

    gpg --dearmor filename.asc

Then, rename the gpg file to::

    key_sig.pub

Where ``sig`` is the signature of the key that you can obtain with::

    gpg -v filename.gpg
    # pub   rsa4096 2019-06-19 [SCA] [expires: 2035-08-05]
    #   C9AA8AC5EF801C50974CAB8E2B4B066FFE3D288E
    # uid           Max Mustermann <max@mustermann.de>
    # sig        2B4B066FFE3D288E 2090-08-10   [selfsig]

The cron jobs are automatically created when running ``ansible-playbook [...] site.yml``.


Removing keys from the DCOR instance
====================================
If you wish to remove a signature, you have to remove the corresponding file
from the current directory (to avoid recreation of the cron job), and manually
remove the cron job matching the signature from ``/etc/cron.d/`` on the host.
