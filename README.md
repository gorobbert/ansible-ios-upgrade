README
======

Updating multiple IOS devices can be a timeconsuming task. The playbooks and roles in this repository can be used to automate this task.
The following steps  will be performed:

- [ios-backup-role] Create Backup of Config and State 
- [ios-upgrade-role] Check if the target device is supported 
- [ios-upgrade-role] Check if there is enough free space on the target device
- [ios-upgrade-role] Transfer image to device using SCP/TFTP
- [ios-upgrade-role] Validate MD5 of transferred image
- [ios-upgrade-role] Configure device to boot new firmware
- [TODO] Optionally reload the device
- [TODO] delete old firmware

SETUP
=====

To succesfully run the playbooks, the Cisco IOS ansible module is needed as well as the paramiko python module.
In addition, if the SCP transfer methode is used the corresponding module is also required. 

    ansible-galaxy collection install cisco.ios
    pip install --user paramiko
    pip install --user scp
    
Obtain the firmware images from Cisco and ensure the checksum of the downloaded files match those on the Cisco website. Place them in the roles/ios-upgrade-role/files directory. This ensures the role can verify the MD5 checksum and push the images using SCP when desired.

RUN
===

In the inventory hosts files, define the routers and switches that should be updated. In this file, the TFTP server IP can also be specified as well as the directory used for backups.
The group_vars directory is used to specify variables for switches and routers in their respective files switches.yml and routers.yml. Both files contain the `firmware_per_model` variable, which is used to specify supported models and their desired firmware images. Also, the desired transfer methode can be specified in these files.

To run just the backup playbook for the routers using the vault pass:

    ansible-playbook -i inventory/hosts playbooks/backup_routers.yml --ask-vault-pass

To run the upgrade playbook for the switches (this will also run the backup task first):

    ansible-playbook -i inventory/hosts playbooks/upgrade_switches.yml --ask-vault-pass


TFTP Server
===========

A simple way to serve the firmware of IOS devices is using TFTP.
To quickly setup a TFTP server, one can use pythons virtual environment feature:

    $ python3 -m venv venv
    $ source venv/bin/activate
    $ pip install py3tftp

Since the default TFTP port is 69, the service needs to be run as the root user.
As root:

    # source venv/bin/activate
    # cd /roles/ios-upgrade-role/files
    # py3tftp -p 69 -v

For production usage, it is better to run a dedicated TFTP server. Either way, ensure that the IOS images are present from the files
