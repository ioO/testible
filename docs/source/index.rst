========
Testible
========

Playbooks to start and stop virtualbox vms to test your playbook.

When you write playbooks you would like to test on virtual machine before. More you learn ansible, more playbooks you 
write. You use snapshots to keep your vm at the right state. It becomes time consuming to start, stop and 
restore your vms.

Here comes **testible**, an easy and *ansible* way to start vm at state, test your playbooks, stop and restore state.

Quick start
===========

Download playbooks
------------------

::

   wget https://raw.githubusercontent.com/ioO/testible/master/start.yml
   wget https://raw.githubusercontent.com/ioO/testible/master/stop.yml


Add *ansible.cfg* and set *inventory*
-------------------------------------

In the directory where you have downloaded playbooks add *ansible.cfg* file and set the inventory path::

   # ansible.cfg
   inventory = ./hosts

.. note::

   Even if your inventory and host vars are in the same folder that the 2 playbooks, define the inventory to *./hosts*.
   Both playbooks that start and stop vm are including vars of vm based on *inventory_dir* var.

Get uuid of virtualbox vm and snapshots
---------------------------------------

To get your vm uuid, run::

   $ VBoxManage list vms

To get your vm snapshots, run::

   $ VBoxManage snapshot uuid list

Set inventory and host_vars
---------------------------

In *hosts* file add localhost and vm::


   localhost ansible_connection=local
   vm_name

For each vm you need to add a var file. In *hosts_vars/vm_name* add::

   ---
   host:
   ip:
     lan: 192.168.56.10
   vbox:
     uuid: vm_uuid
     snapshots:
       snap_name: snapshot_uuid

Start and stop vm
-----------------

To start your vm and restore a snapshot::


   ansible-playbook start.yml  --extra-vars='{"vm": "vm_name", "snapshot": "snap_name"}'

To stop your vm::

   ansible-playbook stop.yml  --extra-vars='{"vm": "vm_name"}'

