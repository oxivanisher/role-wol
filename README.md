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


Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
