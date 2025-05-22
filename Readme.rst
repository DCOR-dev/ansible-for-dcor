DCOR Ansible Control
====================

These are ansible playbooks for setting up and upgrading DCOR on a
vanilla Ubuntu 24.04 machine.

To get started on your control node, install ansible::

    $ pipx install ansible
    installed package ansible 11.6.0, installed using Python 3.12.3

Then, create a virtual machine with a vanilla Ubuntu 24.04 server installation.
Make sure you can SSH into the machine with::

    ssh dcorvm

You can achieve this by adding the following line to your ``~/.ssh/config`` file::

    Host dcorvm
        Hostname 192.168.122.142
        ForwardAgent no
        User root
        IdentityFile ~/.ssh/id_local_vms
        IdentitiesOnly yes

You can create a dedicated key pair and copy it to the VM like so (make
sure you set ``PermitRootLogin yes`` in your VM's ``/etc/ssh/sshd_config`` file)::

    $ ssh-keygen -t ed25519 -f ~/.ssh/id_local_vms
    $ ssh-copy-id -i ~/.ssh/id_local_vms.pub root@192.168.122.142

