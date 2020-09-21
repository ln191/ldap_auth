# Ldap Authentication on linux server

# Work in progress

## Pre-install 

install ldap clinet

sudo apt-get install libnss-ldap libpam-ldap ldap-utils nscd -y

https://www.techrepublic.com/article/how-to-authenticate-a-linux-client-with-ldap-server/

## Playbook

## Files that will be change 

| File                  |   Description                             |   Permission     |
| ----                  |   -----------                             |   ----------     |
|   /etc/ldap.conf      | Setting for ldap connects and search.     | root:root 0644   |
|   /etc/pam.d/common-account | Login Account settings              | root:root 0644   |
|   /etc/pam.d/common-auth    | Login Auth settings                 | root:root 0644   |



## LDAP conf

When ldap client has been install on the linux machine, generated a /etc/ldap.conf with the setting given in the setup process,

This here we control how the ldap server is accesed and what fliters to use when searching the ldap server.

## PAM ( Pluggable authentication module )

Authentecation in linux is control by PAM, when you install ldap client it also install a ldap pam module, and apply it to the PAM configs.

this allows both ldap and local users to login on the server.

The pam configs is found at /etc/pam.d directory. the common files is the base files and is include in the other config files.
There is a config file for each service needing auth, like sshd, login(unix login). osv
so to apply ldap auth to all service we change the commen files.

If you only want a spesific group of ldap to login, you need to change the pam config files, otherwise you will not be able to login with local users.

reset chache    sudo nscd --invalidate=hosts
restart       sudo /etc/init.d/nscd restart´   ||    service nscd restart

### Format

http://linux-pam.org/Linux-PAM-html/sag-configuration-file.html

## Enabled audit logging on session

add to end of /etc/pam.d/commen-session,  will enable audit logging on all user sessions

session required pam_tty_audit.so enable=*

## Give sudoer permission to ldap users

add to /etc/sudoers,  gives sudo permision to ldap group, except sudo su and sudo visudo cmd

%<ldap_group> ALL= ALL, !/bin/su, !/usr/sbin/visudo

