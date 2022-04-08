Role Name
=========

Tiny role to collect and backup IOS facts and config to a file.

Requirements
------------

The Cisco IOS ansible module.

Role Settings
-------------

The role is tested with the following connection types:

- ansible_network_os: ios
- ansible_connection: network_cli

Example Playbook
----------------

    - hosts: switches
      roles:
         - ios-backup-role

License
-------

BSD