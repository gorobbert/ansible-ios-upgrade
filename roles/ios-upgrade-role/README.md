IOS Upgrade Role
================

Upgrade A brief description of the role goes here.

Requirements
------------

The following is required:
- Cisco IOS Ansible Module (ansible-galaxy collection install cisco.ios)
- Paramiko (pip install paramiko)
- SCP (pip install scp) if used to push IOS firmware
- TFTP server if used to serve IOS firmware


Role Variables
--------------

Important variable that needs to be present is 'firmware_per_model'. Use this var to specify the desired firmware image name per device.
If the device is not found in this dict, the play will fail (there is also no target firmware then, so the device model is considered not supported).

The image name is used to get the file from the 'files' directory within the role.
This image is used to calculate the MD5 sum and also when using the SCP method to push firmware.
Otherwise, the image is pulled using TFTP from the root directory.

Files
-----

In the role file directory, make sure the binary firmware images are present that you want to upgrade to.
Make sure their MD5 checksum matches those published on the cisco site.
The role compares the MD5 sum of the transferred IOS image to the IOS image present in the files directory.


License
-------

BSD