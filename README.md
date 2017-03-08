Testible
========

An example how to test playbooks and roles with virtualbox vms

Quick start
-----------

1.  Copy testible-playbook.yml file in your ansible projet in the same
    folder than your playbooks.
2.  Set variables host vars for localhost and virtualbox vm host
3.  Run this command
```shell
ansible-playbook testible-playbook.yml  --extra-vars='{"vm": "name of vm", "snapshot": "name of snapshot", "playbook": "name of playbook"}'
```
Test playbook use **localhost** host. It sets the vm at the snapshot and
launch the vm. It includes your playbook defined in *playbook* extra
vars.

You need to define vm and snapshot uuid, it is explained below.

Configure localhost
-------------------

First you need virtualbox and install at least one vm.

Based on [ansible best
practices](http://docs.ansible.com/ansible/playbooks_best_practices.html),
this example use the directory layout. So variables needed for localhost
are located on *host\_vars/localhost*. The file is commented. The
testible playbook can sit along your project playbook.

The script need your local user in var to configure *$HOME/.ssh/config*
file. The playbook add an entry for the test vm at the start and delete
the entry when it quits.
```yaml
host:
  user:
    local: name of your local user
```

Configure vm
------------

For each vm choose a name. Add it to your inventory file and define
variables in *host\_vars/name\_of\_hosts*.

To get your vm uuid, run :
```shell
$ VBoxManage list vms
```

In your host var file add this var
```yaml
host:
  vbox:
    uuid: vmuuid
```
To get your vm snapshots, run :
```shell
$ VBoxManage snapshot vmuuid list
```
In your host var file add this var.
```yaml
host:
  name: your host name
  vbox:
    uuid: vbox_uuid
    snapshots:
      bare_minimal: snapshotuuid
```
Take a look at host\_vars files ;)
