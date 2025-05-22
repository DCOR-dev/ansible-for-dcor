DCOR Ansible Control
====================

These are ansible playbooks for setting up and upgrading DCOR on a
vanilla Ubuntu 24.04 machine.

To get started on your control node, install ansible::

    $ pipx install ansible
    installed package ansible 11.6.0, installed using Python 3.12.3

Then, create a virtual machine with a vanilla Ubuntu 24.04 server installation.

To give you VM a static IP address run ``virsh net-edit default`` and enter the
values that the VM currently has in the `<host>` tag::

    <network>
      <name>default</name>
      <uuid>d8cd946b-a0a8-409a-f9c4-700204d5741f</uuid>
      <forward mode='nat'>
        <nat>
          <port start='1024' end='65535'/>
        </nat>
      </forward>
      <bridge name='virbr0' stp='on' delay='0'/>
      <mac address='52:52:00:a4:12:8b'/>
      <ip address='192.168.122.1' netmask='255.255.255.0'>
        <dhcp>
          <range start='192.168.122.2' end='192.168.122.254'/>
          <host mac='52:54:00:1e:7a:a2' name='dcorvm' ip='192.168.122.143'/>
        </dhcp>
      </ip>
    </network>

Make sure you can SSH into the machine with::

    ssh dcorvm

You can achieve this by adding the following line to your ``~/.ssh/config`` file::

    Host dcorvm
        Hostname 192.168.122.143
        ForwardAgent no
        User root
        IdentityFile ~/.ssh/id_local_vms
        IdentitiesOnly yes

You can create a dedicated key pair and copy it to the VM like so (make
sure you set ``PermitRootLogin yes`` in your VM's ``/etc/ssh/sshd_config`` file)::

    $ ssh-keygen -t ed25519 -f ~/.ssh/id_local_vms
    $ ssh-copy-id -i ~/.ssh/id_local_vms.pub root@192.168.122.143

