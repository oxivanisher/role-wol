wol
=========

This role configures WOL on the target system so that the system can be started or woken.

Remember: To be able to start the system after a shutdown, you need to whitelist the module from acpi:
Source: https://askubuntu.com/a/1160570


```plaintext
sudo lshw -C network
*-network
   descrição: Ethernet interface
   produto: NetXtreme BCM57765 Gigabit Ethernet PCIe
   fabricante: ...
   nome lógico: enp2s0f0
   ... autonegotiation=on broadcast=yes ***driver=tg3***
```
after this, knowing that the driver is tg3:

```plaintext
sudo vim /etc/default/acpi-support
```

I changed the
```plaintext
# Add modules to this list to leave them in the kernel over suspend/resume
MODULES_WHITELIST=""
```
to
`MODULES_WHITELIST="tg3"`
And after a restart of the network service it worked like a charm, but only with Magic Packets, that is cool; to provide that Magic Packets I'm using a python script, adapted from here; I said adapted because this script is for Python 2, and my setup is Python 3. Hope this helps someone.

Role Variables
--------------

None.

Example Playbook
----------------
```yaml
- name: Configure WOL
  hosts: clients
  roles:
    - role: oxivanisher.linux_desktop.wol
```

License
-------

BSD

Author Information
------------------

This role is part of the [oxivanisher.linux_desktop](https://galaxy.ansible.com/ui/repo/published/oxivanisher/linux_desktop/) collection, and the source for that is located on [github](https://github.com/oxivanisher/collection-linux_desktop).
