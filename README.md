# Failed to validate GPG signature for X

This is an example of how installing a local RPM file fails reliably on a "fresh" machine.

## Start Vagrant

This will automatically download the RPM and run Ansible.

```bash
vagrant up
```

## See failure

```
==> node1: Running provisioner: ansible...
    node1: Running ansible-playbook...

PLAY [Install Telegraf] ********************************************************

TASK [Gathering Facts] *********************************************************
ok: [node1]

TASK [install influx gpg key] **************************************************
changed: [node1]

TASK [install telegraf packages] ***********************************************
fatal: [node1]: FAILED! => {"changed": false, "msg": "Failed to validate GPG signature for telegraf-1.13.4-1.x86_64"}

PLAY RECAP *********************************************************************
node1                      : ok=2    changed=1    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0

Ansible failed to complete successfully. Any error output should be
visible above. Please fix these errors and try again.
```

## Login to the machine, and do 2 tests, where verification succeeds

From host machine

```bash
vagrant ssh
```

From vagrant

```bash
[rocky@node1 ~]$ sudo -s
[root@node1 rocky rpm -K /tmp/telegraf-1.13.4-1.x86_64.rpm
/tmp/telegraf-1.13.4-1.x86_64.rpm: digests OK
[root@node1 rocky dnf localinstall /tmp/telegraf-1.13.4-1.x86_64.rpm
Failed to set locale, defaulting to C.UTF-8
Last metadata expiration check: 1:26:23 ago on Fri Oct  8 14:40:58 2021.
Dependencies resolved.
======
 Package                                                    Architecture                                             Version                                                       Repository                                                      Size
======
Installing:
 telegraf                                                   x86_64                                                   1.13.4-1                                                      @commandline                                                    19 M

Transaction Summary
======
Install  1 Package

Total size: 19 M
Installed size: 65 M
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                                1/1
  Running scriptlet: telegraf-1.13.4-1.x86_64                                                                                                                                                                                                       1/1
  Installing       : telegraf-1.13.4-1.x86_64                                                                                                                                                                                                       1/1
  Running scriptlet: telegraf-1.13.4-1.x86_64                                                                                                                                                                                                       1/1
Created symlink /etc/systemd/system/multi-user.target.wants/telegraf.service → /usr/lib/systemd/system/telegraf.service.

  Verifying        : telegraf-1.13.4-1.x86_64                                                                                                                                                                                                       1/1

Installed:
  telegraf-1.13.4-1.x86_64

Complete!
```

## Destroy your Vagrant

```bash
vagrant destroy
```
