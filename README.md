hetzner_installimage
====================

[![Build Status](https://travis-ci.org/andrelohmann/ansible-role-hetzner_installimage.svg?branch=master)](https://travis-ci.org/andrelohmann/ansible-role-hetzner_installimage)

Use this role to base provision your hetzner machines with the hetzner installimage script and your public ssh key (but all automated through ansible).

Requirements
------------

This role requires you to have a server at hetzner.de and some api credentials as well as your provisioning key uploaded to the hetzner robot. Read more about that under https://wiki.hetzner.de/index.php/Robot_Webservice

Also for security reasons, the role will not reboot and install the machine, if it can't access it and check for the existance of a hostcode file. Therefor you need to have your provisioning key allready installed on the machine(s) you want to install (can be done through robot).

Config Variables
---------------

The following variables are suggested to be set within your ansible.cfg file

    [defaults]
    inventory = __YOUR_INVENTORY__
    forks = 1
    host_key_checking = false
    private_key_file = __YOUR_HETZNER_PROVISIONING_KEY__
    remote_user = root
    roles_path = __PATH_TO_YOUR_GALAXY_ROLES__
    [ssh_connection]
    pipelining = True
    scp_if_ssh = True
    control_path = %(directory)s/%%h-%%r

Role Variables
--------------

The default set of variables defines the installimage and needs at best to be overwritten in group_vars/host_vars

    hetzner_installimage_install_drives:
    - DRIVE1 /dev/sda
    - DRIVE2 /dev/sdb
    hetzner_installimage_install_raid:
    - SWRAID 1
    - SWRAIDLEVEL 1
    hetzner_installimage_install_bootloader: grub
    hetzner_installimage_install_hostname: Debian-87-jessie-64-minimal
    hetzner_installimage_install_partitions:
    - PART swap swap 32G
    - PART /boot ext4 1G
    - PART / ext4 all
    hetzner_installimage_install_image: /root/.oldroot/nfs/images/Debian-87-jessie-64-minimal.tar.gz
    #hetzner_installimage_install_image: /root/.oldroot/nfs/images/Ubuntu-1404-trusty-64-minimal.tar.gz
    #hetzner_installimage_install_image: /root/.oldroot/nfs/images/Ubuntu-1604-xenial-64-minimal.tar.gz

The following mandatory variables need to be set in group_vars/host_vars to allow communication with the webservice and deployment of the public key

    hetzner_installimage_webservice_username: username
    hetzner_installimage_webservice_password: password
    hetzner_installimage_key_fingerprint: 00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff

The following variable can be set optionally, to set the hostname within the hetzner robot

    hetzner_server_name: __YOUR_SERVER_NAME__



Example Playbook
----------------

    - hosts: hetzner
      roles:
         - { role: andrelohmann.hetzner_installimage }

Installation Steps
------------------

  * Install a new machine
    1. Enter your hetzner robot (robot.your-server.de)
    2. Order a new server
    3. Select your operating system
    4. Select your provisioning key
    5. Run the hetzner_installimge role
  * Install an existing machine
    1. Enter your hetzner robot (robot.your-server.de)
    2. Select the machine
    3. Select "Linux"
    4. Select your provisioning key
    5. Reset the machine
    6. Run the hetzner_installimge role
  * Install an allready provisioned machine
    1. Enter the machine
    2. Delete /etc/hostcode
    3. Run the hetzner_installimge role

If you are sure, you will not accidentally purge a running machine, allready in use, you can directly run the role with the extra variable **--extra-vars "{ hetzner_installimage_ignore_hostcode: True }"**. This way the role will not check the machine for an existing /etc/hostcode file but will also not prevent the machine from being purged accidentally!

License
-------

MIT

Author Information
------------------

https://github.com/andrelohmann
