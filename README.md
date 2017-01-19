hetzner_installimage
====================

[![Build Status](https://travis-ci.org/andrelohmann/ansible-role-hetzner_installimage.svg?branch=master)](https://travis-ci.org/andrelohmann/ansible-role-hetzner_installimage)

Use this role to base provision your hetzner machines with the hetzner installimage script and your public ssh key (but all automated through ansible).

Requirements
------------

This role requires you to have a server at hetzner.de and some api credentials as well as your provisioning key uploaded to the hetzner robot. Read more about that under https://wiki.hetzner.de/index.php/Robot_Webservice

Config Variables
---------------

The following variables are suggested to be set within you ansible.cfg file

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
    hetzner_installimage_install_hostname: Debian-86-jessie-64-minimal
    hetzner_installimage_install_partitions:
    - PART swap swap 32G
    - PART /boot ext4 1G
    - PART / ext4 all
    hetzner_installimage_install_image: /root/.oldroot/nfs/images/Debian-86-jessie-64-minimal.tar.gz

The following mandatory variables need to be set in group_vars/host_vars to allow communication with the webservice and deployment of the public key

    hetzner_installimage_webservice_username: username
    hetzner_installimage_webservice_password: password
    hetzner_installimage_key_fingerprint: 00:11:22:33:44:55:66:77:88:99:aa:bb:cc:dd:ee:ff



Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: hetzner
      roles:
         - { role: andrelohmann.ansible-role-hetzner_installimage }

License
-------

MIT

Author Information
------------------

https://github.com/andrelohmann
