Staging environment with virtual machine
========================================

Create a virtual machine with a vanilla Ubuntu 24.04 server installation using
e.g. "Virtual Machine Manager".

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

Make sure you can SSH into the machine with (make sure you set
``PermitRootLogin yes`` in your VM's ``/etc/ssh/sshd_config`` file)::

    ssh root@192.168.122.143

Create a dedicated SSH key pair and copy it to the VM like so ::

    $ ssh-keygen -t ed25519 -f ~/.ssh/id_local_vms
    $ ssh-copy-id -i ~/.ssh/id_local_vms.pub root@192.168.122.143

Add an entry to your ``~/.ssh/config`` file so that ansible can just run
``ssh 192.168.122.143`` to get in::

    Host 192.168.122.143
        ForwardAgent no
        User root
        IdentityFile ~/.ssh/id_local_vms
        IdentitiesOnly yes
