# LDAP

## openldap
- [OpenLDAP, Main Page](https://www.openldap.org/)
- よく使うコマンド
  - `ldapsearch -Q -LLL -Y EXTERNAL -H ldapi:/// -b cn=config dn`
  - `ldapsearch -Q -LLL -Y EXTERNAL -H ldapi:/// -b dc=example,dc=com`
  - `ldapadd -x -W -D {execrole_path} -f {ldif_file}`
  - `ldapdelete -x -D {execrole_path} -W {path}`
    - `ldapdelete -x -D cn=Manager,dc=example,dc=com -W uid=user1,ou=users,dc=example,dc=com`

## 基本的なobjectsの登録example
```
root@ldaptest:/home/labuser# cat example_com.ldif
dn: dc=example,dc=com
objectClass: top
objectClass: dcObject
objectclass: organization
o: example domain
dc: example

dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
cn: Manager

dn: ou=users,dc=example,dc=com
objectClass: organizationalUnit
ou: users
root@ldaptest:/home/labuser#
root@ldaptest:/home/labuser# sudo ldapadd -x -D cn=Manager,dc=example,dc=com -W -f example_com.ldif
Enter LDAP Password:
adding new entry "dc=example,dc=com"

adding new entry "cn=Manager,dc=example,dc=com"

adding new entry "ou=users,dc=example,dc=com"
```
- groupとかsudoRoleとか
```
dn: cn=sudo,ou=groups,dc=example,dc=com
objectClass: posixGroup
cn: sudo
gidNumber: 5001
memberUid: testUser1

dn: cn=users,ou=groups,dc=example,dc=com
objectClass: posixGroup
cn: users
gidNumber: 5000
memberUid: testUser1
memberUid: testUser2

dn: cn=%sudo,ou=sudoers,dc=example,dc=com
cn: %sudo
objectClass: sudoRole
objectClass: top
sudoCommand: ALL
sudoHost: ALL
sudoUser: %sudo
```

## ldapで管理するuserにのっけておくべきなpractice
- example
```
labuser@ldaptest:~$ cat testuser1_users_example_com.ldif
dn: uid=testuser1,ou=users,dc=example,dc=com
uid: testuser1
#objectClass: account
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: ldapPublicKey
#objectClass: radiusprofile
objectClass: top
cn: testuser1
#gn: user1
givenName: user1
sn: test
uidNumber: 5000
gidNumber: 5000
homeDirectory: /home/testuser1
gecos: Test User1
loginShell: /bin/bash
mail: testuser1@example.com
#mailRecipient: testuser1@example.com
sshPublicKey: xxxxx
userPassword: {SSHA}xxxxx
```

## 概ねどんなツリーだとよさそうか
```
dc=example,dc=com
├── cn=manager
├── cn=sssd
├── ou=users
│   ├── uid=user1
│   └── uid=user2
├── ou=groups
│   ├── cn=group1
│   └── cn=group2
└── ou=sudoers
    ├── cn=%adm
    ├── cn=%sudo
    ├── cn=%wheel
    └── cn=%group1
```
ユーザのみであればだいたいこんなもんでいいのではなかろうか．機器系が入る場合は別にserver/networkなどを分けてあげればよさそう．
ちなみにsssdはデフォルトでsudoは `ldap_search_base` 配下のsudoers配下を見る．これとは異なるsudoers dir構成にするなら `sssd.conf` で明示指定( `ldap_sudo_search_base` )が必要．

### openldap install buttle on ubuntu18.04
```
root@ldaptest:/home/labuser# sudo apt install slapd ldap-utils
Reading state information... Done
The following package was automatically installed and is no longer required:
  libdumbnet1
Use 'sudo apt autoremove' to remove it.
The following additional packages will be installed:
  libltdl7 libodbc1
Suggested packages:
  libsasl2-modules-gssapi-mit | libsasl2-modules-gssapi-heimdal libmyodbc odbc-postgresql tdsodbc unixodbc-bin
The following NEW packages will be installed:
  ldap-utils libltdl7 libodbc1 slapd
0 upgraded, 4 newly installed, 0 to remove and 0 not upgraded.
Need to get 1,731 kB of archives.
After this operation, 17.6 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu bionic/main amd64 libltdl7 amd64 2.4.6-2 [38.8 kB]
Get:2 http://archive.ubuntu.com/ubuntu bionic/main amd64 libodbc1 amd64 2.3.4-1.1ubuntu3 [183 kB]
Get:3 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 slapd amd64 2.4.45+dfsg-1ubuntu1.5 [1,385 kB]
Get:4 http://archive.ubuntu.com/ubuntu bionic-updates/main amd64 ldap-utils amd64 2.4.45+dfsg-1ubuntu1.5 [125 kB]
Fetched 1,731 kB in 2s (756 kB/s)
Preconfiguring packages ...
Selecting previously unselected package libltdl7:amd64.
(Reading database ... 139694 files and directories currently installed.)
Preparing to unpack .../libltdl7_2.4.6-2_amd64.deb ...
Unpacking libltdl7:amd64 (2.4.6-2) ...
Selecting previously unselected package libodbc1:amd64.
Preparing to unpack .../libodbc1_2.3.4-1.1ubuntu3_amd64.deb ...
Unpacking libodbc1:amd64 (2.3.4-1.1ubuntu3) ...
Selecting previously unselected package slapd.
Preparing to unpack .../slapd_2.4.45+dfsg-1ubuntu1.5_amd64.deb ...
Unpacking slapd (2.4.45+dfsg-1ubuntu1.5) ...
Selecting previously unselected package ldap-utils.
Preparing to unpack .../ldap-utils_2.4.45+dfsg-1ubuntu1.5_amd64.deb ...
Unpacking ldap-utils (2.4.45+dfsg-1ubuntu1.5) ...
Setting up ldap-utils (2.4.45+dfsg-1ubuntu1.5) ...
Setting up libltdl7:amd64 (2.4.6-2) ...
Setting up libodbc1:amd64 (2.3.4-1.1ubuntu3) ...
Setting up slapd (2.4.45+dfsg-1ubuntu1.5) ...
  Creating new user openldap... done.
  Creating initial configuration... done.
  Creating LDAP directory... done.
Processing triggers for ufw (0.36-0ubuntu0.18.04.1) ...
Processing triggers for ureadahead (0.100.0-21) ...
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for systemd (237-3ubuntu10.41) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
root@ldaptest:/home/labuser#
```
ここまででインストール自体は完了

- その後のいろいろ
```
# いまどんなconfig ldifがあるのか
root@ldaptest:/home/labuser# sudo ls -l /etc/ldap/slapd.d/cn\=config
total 28
-rw------- 1 openldap openldap  436 Jun 18 18:09 'cn=module{0}.ldif'
drwxr-x--- 2 openldap openldap 4096 Jun 18 18:09 'cn=schema'
-rw------- 1 openldap openldap  378 Jun 18 18:09 'cn=schema.ldif'
-rw------- 1 openldap openldap  396 Jun 18 18:09 'olcBackend={0}mdb.ldif'
-rw------- 1 openldap openldap  513 Jun 18 18:09 'olcDatabase={0}config.ldif'
-rw------- 1 openldap openldap  657 Jun 18 18:09 'olcDatabase={-1}frontend.ldif'
-rw------- 1 openldap openldap  926 Jun 18 18:09 'olcDatabase={1}mdb.ldif'

# slapd statusがあがってるかみる
root@ldaptest:/home/labuser# systemctl status slapd
● slapd.service - LSB: OpenLDAP standalone server (Lightweight Directory Access Protocol)
   Loaded: loaded (/etc/init.d/slapd; generated)
  Drop-In: /lib/systemd/system/slapd.service.d
           └─slapd-remain-after-exit.conf
   Active: active (running) since Thu 2020-06-18 18:09:26 UTC; 12min ago
     Docs: man:systemd-sysv-generator(8)
    Tasks: 3 (limit: 4915)
   CGroup: /system.slice/slapd.service
           └─5825 /usr/sbin/slapd -h ldap:/// ldapi:/// -g openldap -u openldap -F /etc/ldap/slapd.d

Jun 18 18:09:26 ldaptest systemd[1]: Starting LSB: OpenLDAP standalone server (Lightweight Directory Access Protocol)...
Jun 18 18:09:26 ldaptest slapd[5804]:  * Starting OpenLDAP slapd
Jun 18 18:09:26 ldaptest slapd[5822]: @(#) $OpenLDAP: slapd  (Ubuntu) (May  1 2020 17:11:02) $
                                            Debian OpenLDAP Maintainers <pkg-openldap-devel@lists.alioth.debian.org>
Jun 18 18:09:26 ldaptest slapd[5825]: slapd starting
Jun 18 18:09:26 ldaptest slapd[5804]:    ...done.
Jun 18 18:09:26 ldaptest systemd[1]: Started LSB: OpenLDAP standalone server (Lightweight Directory Access Protocol).

# /etc/ldap配下にはconfやらpreinstallなschemaもいくつかある
# slapd.dがldap directoryかな．
root@ldaptest:/home/labuser# ls -la /etc/ldap/
total 24
drwxr-xr-x  5 root     root     4096 Jun 18 18:09 .
drwxr-xr-x 95 root     root     4096 Jun 18 18:09 ..
-rw-r--r--  1 root     root      332 Jul 26  2019 ldap.conf
drwxr-xr-x  2 root     root     4096 May  1 17:11 sasl2
drwxr-xr-x  2 root     root     4096 Jun 18 18:09 schema
drwxr-xr-x  3 openldap openldap 4096 Jun 18 18:09 slapd.d

# ldapsearchで現状のdnをみる．
root@ldaptest:/home/labuser# sudo ldapsearch -Q -LLL -Y EXTERNAL -H ldapi:/// -b cn=config dn
dn: cn=config

dn: cn=module{0},cn=config

dn: cn=schema,cn=config

dn: cn={0}core,cn=schema,cn=config

dn: cn={1}cosine,cn=schema,cn=config

dn: cn={2}nis,cn=schema,cn=config

dn: cn={3}inetorgperson,cn=schema,cn=config

dn: olcBackend={0}mdb,cn=config

dn: olcDatabase={-1}frontend,cn=config

dn: olcDatabase={0}config,cn=config

dn: olcDatabase={1}mdb,cn=config

# slapcatすれば現状入ってるすべての構成がみれる
root@ldaptest:/home/labuser# sudo slapcat
dn: dc=nodomain
objectClass: top
objectClass: dcObject
objectClass: organization
o: nodomain
dc: nodomain
structuralObjectClass: organization
entryUUID: xxx
creatorsName: cn=admin,dc=nodomain
createTimestamp: 20200618180925Z
entryCSN: 20200618180925.000000Z#000000#000#000000
modifiersName: cn=admin,dc=nodomain
modifyTimestamp: 20200618180925Z

dn: cn=admin,dc=nodomain
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator
userPassword:: xxxx
structuralObjectClass: organizationalRole
entryUUID: xxxx
creatorsName: cn=admin,dc=nodomain
createTimestamp: 20200618180925Z
entryCSN: 20200618180925.000000Z#000000#000#000000
modifiersName: cn=admin,dc=nodomain
modifyTimestamp: 20200618180925Z

# ドメイン名を変更する
root@ldaptest:/home/labuser# cat chdomain.ldif
dn: olcDatabase={1}mdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=example,dc=com

dn: olcDatabase={1}mdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=Manager,dc=example,dc=com

root@ldaptest:/home/labuser# sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f chdomain.ldif
SASL/EXTERNAL authentication started
SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
SASL SSF: 0
modifying entry "olcDatabase={1}mdb,cn=config"

modifying entry "olcDatabase={1}mdb,cn=config"

root@ldaptest:/home/labuser#

# オブジェクト登録
root@ldaptest:/home/labuser# cat example_com.ldif
dn: dc=example,dc=com
objectClass: top
objectClass: dcObject
objectclass: organization
o: example domain
dc: example

dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
cn: Manager

dn: ou=users,dc=example,dc=com
objectClass: organizationalUnit
ou: users
root@ldaptest:/home/labuser#
root@ldaptest:/home/labuser# sudo ldapadd -x -D cn=Manager,dc=example,dc=com -W -f example_com.ldif
Enter LDAP Password:
adding new entry "dc=example,dc=com"

adding new entry "cn=Manager,dc=example,dc=com"

adding new entry "ou=users,dc=example,dc=com"

root@ldaptest:/home/labuser# ldapsearch -Q -LLL -Y EXTERNAL -H ldapi:/// -b dc=example,dc=com
dn: dc=example,dc=com
objectClass: top
objectClass: dcObject
objectClass: organization
o: example domain
dc: example

dn: ou=users,dc=example,dc=com
objectClass: organizationalUnit
ou: users

dn: cn=Manager,dc=example,dc=com
objectClass: organizationalRole
cn: Manager

labuser@ldaptest:~$ cat add_user.ldif
dn: uid=testuser1,ou=users,dc=example,dc=com
uid: testuser1
objectClass: account
#objectClass: userAccount
objectClass: posixAccount
#objectClass: radiusprofile
objectClass: top
cn: testuser1
#gn: user1
#sn: test
uidNumber: 5000
gidNumber: 5000
homeDirectory: /home/testuser1
gecos: Test User1
loginShell: /bin/bash
#mail: testuser1@example.com
#mailRecipient: testuser1@example.com

labuser@ldaptest:~$ ldapadd -x -W -D "cn=Manager,dc=example,dc=com" -f add_user.ldif
Enter LDAP Password:
adding new entry "uid=testuser1,ou=users,dc=example,dc=com"

labuser@ldaptest:~$ ldapsearch -x -D cn=Manager,dc=example,dc=com -W -b ou=users,dc=example,dc=com
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <ou=users,dc=example,dc=com> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# users, example.com
dn: ou=users,dc=example,dc=com
objectClass: organizationalUnit
ou: users

# testuser1, users, example.com
dn: uid=testuser1,ou=users,dc=example,dc=com
uid: testuser1
objectClass: account
objectClass: posixAccount
objectClass: top
cn: testuser1
uidNumber: 5000
gidNumber: 5000
homeDirectory: /home/testuser1
gecos: Test User1
loginShell: /bin/bash

# search result
search: 2
result: 0 Success

# numResponses: 3
# numEntries: 2

# openssh ldifを入れる．これをいれると `objectClass: ldapPublicKey`, `sshPublicKey` 属性が使えるようになる．
labuser@ldaptest:~$ wget https://www.osstech.co.jp/~hamano/posts/centos7-openldap-ssh/openssh-lpk-openldap.ldif
--2020-07-26 12:55:12--  https://www.osstech.co.jp/~hamano/posts/centos7-openldap-ssh/openssh-lpk-openldap.ldif
Resolving www.osstech.co.jp (www.osstech.co.jp)... 153.120.181.221, 2401:2500:203:16:153:120:181:221
Connecting to www.osstech.co.jp (www.osstech.co.jp)|153.120.181.221|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 446
Saving to: ‘openssh-lpk-openldap.ldif’

openssh-lpk-openldap.l 100%[=========================>]     446  --.-KB/s    in 0s

2020-07-26 12:55:13 (14.1 MB/s) - ‘openssh-lpk-openldap.ldif’ saved [446/446]

labuser@ldaptest:~$ sudo mv openssh-lpk-openldap.ldif /etc/ldap/schema/
labuser@ldaptest:~$ cat /etc/ldap/schema/openssh-lpk-openldap.ldif
dn: cn=openssh-lpk-openldap,cn=schema,cn=config
objectClass: olcSchemaConfig
cn: openssh-lpk-openldap
olcAttributeTypes: ( 1.3.6.1.4.1.24552.500.1.1.1.13 NAME 'sshPublicKey' DES
 C 'MANDATORY: OpenSSH Public key' EQUALITY octetStringMatch SYNTAX 1.3.6.1.4.
 1.1466.115.121.1.40 )
olcObjectClasses: ( 1.3.6.1.4.1.24552.500.1.1.2.0 NAME 'ldapPublicKey' DESC
  'MANDATORY: OpenSSH LPK objectclass' SUP top AUXILIARY MUST ( sshPublicKey $
  uid ) )

labuser@ldaptest:~$ sudo ldapadd -Q -Y EXTERNAL -H ldapi:// -f /etc/ldap/schema/openssh-lpk-openldap.ldif
adding new entry "cn=openssh-lpk-openldap,cn=schema,cn=config"
```

## sssを使う
- follow this: https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/configure-ldap-client-on-ubuntu-16-04-debian-8.html
```
labuser@ldaptest:~$ sudo apt install sssd libpam-sss libnss-sss libnss-ldap
[sudo] password for labuser:
Reading package lists... Done
Building dependency tree
Reading state information... Done
labuser@ldaptest:~$
labuser@ldaptest:~$ sudo vi /etc/pam.d/common-session
labuser@ldaptest:~$ sudo cat /etc/pam.d/common-session
cat: sudo: No such file or directory
cat: vi: No such file or directory
#
# /etc/pam.d/common-session - session-related modules common to all services
#
# This file is included from other service-specific PAM config files,
# and should contain a list of modules that define tasks to be performed
# at the start and end of sessions of *any* kind (both interactive and
# non-interactive).
#
# As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
# To take advantage of this, it is recommended that you configure any
# local modules either before or after the default block, and use
# pam-auth-update to manage selection of other modules.  See
# pam-auth-update(8) for details.

# これを利用するsssクライアント側で記述する．これをするとhomedirectoryがログイン時自動生成される．
session required        pam_mkhomedir.so skel=/etc/skel umask=077

# here are the per-package modules (the "Primary" block)
session [default=1]     pam_permit.so
# here's the fallback if no module succeeds
session requisite     pam_deny.so
# prime the stack with a positive return value if there isn't one already;
# this avoids us returning an error just because nothing sets a success code
# since the modules above will each just jump around
session required      pam_permit.so
# The pam_umask module will set the umask according to the system default in
# /etc/login.defs and user settings, solving the problem of different
# umask settings with different shells, display managers, remote sessions etc.
# See "man pam_umask".
session optional      pam_umask.so
# and here are more per-package modules (the "Additional" block)
session required  pam_unix.so
session optional      pam_ldap.so
session optional  pam_systemd.so
# end of pam-auth-update config
# ldap auth configを設定して利用するldapのconfigを実施する．
labuser@ldaptest:~$ sudo dpkg-reconfigure ldap-auth-config
labuser@ldaptest:~$ getent passwd testuser1
testuser1:*:5000:5000:Test User1:/home/testuser1:/bin/bash

# /etc/sshd/sshd_cofnigにこれを追加．するとpubkey認証がつかえるようになる．
AuthorizedKeysCommand /usr/bin/sss_ssh_authorizedkeys
AuthorizedKeysCommandUser root
```
- ref: [EC2上のubuntu16にOpenLDAPを設定した。 - Qiita](https://qiita.com/daichi_fukui/items/2a69de159d683b57fc7f)
- ref: [Configure LDAP Client on Ubuntu 16.04 / Debian 8 - ITzGeek](https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/configure-ldap-client-on-ubuntu-16-04-debian-8.html)

## sssを利用してhome directory自動生成するときはこれ
```
# /etc/pam.d/common-session
#session required pam_mkhomedir.so skel=/etc/skel/ umask=0022
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```

## uidとかgui，ユニーク制約つけたい
- 結局まだわかってないが下記あたりヒント．
```
overlay unique
       unique_uri ldap:///?mail?sub?

```

## sudo schema の設定
```
#Sudo schema for OpenLDAP
#The following schema, in OpenLDAP format, is included with sudo source and binary distributions as schema.OpenLDAP. Simply copy it to the schema directory (e.g. /etc/openldap/schema), add the proper include line in slapd.conf and restart slapd.

attributetype ( 1.3.6.1.4.1.15953.9.1.1
   NAME 'sudoUser'
   DESC 'User(s) who may  run sudo'
   EQUALITY caseExactIA5Match
   SUBSTR caseExactIA5SubstringsMatch
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.2
   NAME 'sudoHost'
   DESC 'Host(s) who may run sudo'
   EQUALITY caseExactIA5Match
   SUBSTR caseExactIA5SubstringsMatch
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.3
   NAME 'sudoCommand'
   DESC 'Command(s) to be executed by sudo'
   EQUALITY caseExactIA5Match
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.4
   NAME 'sudoRunAs'
   DESC 'User(s) impersonated by sudo'
   EQUALITY caseExactIA5Match
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.5
   NAME 'sudoOption'
   DESC 'Options(s) followed by sudo'
   EQUALITY caseExactIA5Match
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.6
   NAME 'sudoRunAsUser'
   DESC 'User(s) impersonated by sudo'
   EQUALITY caseExactIA5Match
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.7
   NAME 'sudoRunAsGroup'
   DESC 'Group(s) impersonated by sudo'
   EQUALITY caseExactIA5Match
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )

attributetype ( 1.3.6.1.4.1.15953.9.1.8
   NAME 'sudoNotBefore'
   DESC 'Start of time interval for which the entry is valid'
   EQUALITY generalizedTimeMatch
   ORDERING generalizedTimeOrderingMatch
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.24 )

attributetype ( 1.3.6.1.4.1.15953.9.1.9
   NAME 'sudoNotAfter'
   DESC 'End of time interval for which the entry is valid'
   EQUALITY generalizedTimeMatch
   ORDERING generalizedTimeOrderingMatch
   SYNTAX 1.3.6.1.4.1.1466.115.121.1.24 )

attributeTypes ( 1.3.6.1.4.1.15953.9.1.10
    NAME 'sudoOrder'
    DESC 'an integer to order the sudoRole entries'
    EQUALITY integerMatch
    ORDERING integerOrderingMatch
    SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 )

objectclass ( 1.3.6.1.4.1.15953.9.2.1 NAME 'sudoRole' SUP top STRUCTURAL
   DESC 'Sudoer Entries'
   MUST ( cn )
   MAY ( sudoUser $ sudoHost $ sudoCommand $ sudoRunAs $ sudoRunAsUser $
   sudoRunAsGroup $ sudoOption $ sudoNotBefore $ sudoNotAfter $
   sudoOrder $ description )
   )
```

- sudoRole.ldif
```
labuser@ldaptest:~$ sudo cat sudoRole.ldif
# AUTO-GENERATED FILE - DO NOT EDIT!! Use ldapmodify.
# CRC32 f877c9ae
dn: cn=sudoRole,cn=schema,cn=config
objectClass: olcSchemaConfig
cn: sudoRole
olcAttributeTypes: {0}( 1.3.6.1.4.1.15953.9.1.1 NAME 'sudoUser' DESC 'User(s
 ) who may  run sudo' EQUALITY caseExactIA5Match SUBSTR caseExactIA5Substrin
 gsMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )
olcAttributeTypes: {1}( 1.3.6.1.4.1.15953.9.1.2 NAME 'sudoHost' DESC 'Host(s
 ) who may run sudo' EQUALITY caseExactIA5Match SUBSTR caseExactIA5Substring
 sMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.26 )
olcAttributeTypes: {2}( 1.3.6.1.4.1.15953.9.1.3 NAME 'sudoCommand' DESC 'Com
 mand(s) to be executed by sudo' EQUALITY caseExactIA5Match SYNTAX 1.3.6.1.4
 .1.1466.115.121.1.26 )
olcAttributeTypes: {3}( 1.3.6.1.4.1.15953.9.1.4 NAME 'sudoRunAs' DESC 'User(
 s) impersonated by sudo (deprecated)' EQUALITY caseExactIA5Match SYNTAX 1.3
 .6.1.4.1.1466.115.121.1.26 )
olcAttributeTypes: {4}( 1.3.6.1.4.1.15953.9.1.5 NAME 'sudoOption' DESC 'Opti
 ons(s) followed by sudo' EQUALITY caseExactIA5Match SYNTAX 1.3.6.1.4.1.1466
 .115.121.1.26 )
olcAttributeTypes: {5}( 1.3.6.1.4.1.15953.9.1.6 NAME 'sudoRunAsUser' DESC 'U
 ser(s) impersonated by sudo' EQUALITY caseExactIA5Match SYNTAX 1.3.6.1.4.1.
 1466.115.121.1.26 )
olcAttributeTypes: {6}( 1.3.6.1.4.1.15953.9.1.7 NAME 'sudoRunAsGroup' DESC '
 Group(s) impersonated by sudo' EQUALITY caseExactIA5Match SYNTAX 1.3.6.1.4.
 1.1466.115.121.1.26 )
olcAttributeTypes: {7}( 1.3.6.1.4.1.15953.9.1.8 NAME 'sudoNotBefore' DESC 'S
 tart of time interval for which the entry is valid' EQUALITY generalizedTim
 eMatch ORDERING generalizedTimeOrderingMatch SYNTAX 1.3.6.1.4.1.1466.115.12
 1.1.24 )
olcAttributeTypes: {8}( 1.3.6.1.4.1.15953.9.1.9 NAME 'sudoNotAfter' DESC 'En
 d of time interval for which the entry is valid' EQUALITY generalizedTimeMa
 tch ORDERING generalizedTimeOrderingMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1
 .24 )
olcAttributeTypes: {9}( 1.3.6.1.4.1.15953.9.1.10 NAME 'sudoOrder' DESC 'an i
 nteger to order the sudoRole entries' EQUALITY integerMatch ORDERING intege
 rOrderingMatch SYNTAX 1.3.6.1.4.1.1466.115.121.1.27 )
olcObjectClasses: {0}( 1.3.6.1.4.1.15953.9.2.1 NAME 'sudoRole' DESC 'Sudoer
 Entries' SUP top STRUCTURAL MUST cn MAY ( sudoUser $ sudoHost $ sudoCommand
  $ sudoRunAs $ sudoRunAsUser $ sudoRunAsGroup $ sudoOption $ sudoOrder $ su
 doNotBefore $ sudoNotAfter $ description ) )
 ```
  - つくりかたは
    - `apt install sudo-ldap`
    - `vim sudo.conf`
      ```
      include /usr/share/doc/sudo-ldap/schema.OpenLDAP`
      ```
    - `slaptest -f sudo.conf -F .`
    - `vim cn\=config/cn\=schema/cn\=\{0\}schema.ldif`
      - てきとうにかきかえる
      ```
      dn: cn=sudoRole,cn=schema,cn=config
      objectClass: olcSchemaConfig
      cn: sudoRole
      olcAttributeTypes: ***(いろいろあるはず)
      olcObjectClasses: ***(sudoRoleのやつが1コあるはず)
      ```
    - 属性はこれ以外にもたせない．末尾にいろいろついてるけどいらないので消す
    - `ldapadd -Q -Y EXTERNAL -H ldapi:// -f sudoRole.ldif` でいれる．これでsudoRoleがつかえるようになる．
- example
```
dn: cn=%sudo,ou=SUDOers,dc=example,dc=com
cn: %sudo
objectclass: top
objectclass: sudoRole
sudocommand: ALL
sudohost: ALL
sudooption: !authenticate
sudorunasuser: ALL
sudouser: %sudo
```
  - sudooptionをたとえば `!authenticate` とするとパスワード認証なしで実行できたりする．他にもoptionは様々ある．

- sssでsudoを使う場合には `sssd.conf` で `sudo_provider   = ldap` などと指定する必要あり．
```
root@sssclient:/home/labuser# cat /etc/sssd/sssd.conf
#[sssd]
#services = nss, pam, ssh
#config_file_version = 2
#domains = default
#
#[sudo]
#
#[nss]
#
#[pam]
#offline_credentials_expiration = 60
#
#[domain/default]
##ldap_id_use_start_tls = True
#cache_credentials = True
#ldap_search_base = dc=example,dc=com
#id_provider = ldap
#auth_provider = ldap
#chpass_provider = ldap
#access_provider = ldap
#ldap_uri = ldap:///
#ldap_default_bind_dn = cn=Manager,dc=example,dc=com
#ldap_default_authtok = passwd
##ldap_tls_reqcert = demand
##ldap_tls_cacert = /etc/ssl/certs/cacert.crt
##ldap_tls_cacertdir = /etc/ssl/certs
#ldap_search_timeout = 50
#ldap_network_timeout = 60
#ldap_access_order = filter
#ldap_access_filter = (objectClass=posixAccount)
#
[sssd]

debug_level         = 0
config_file_version = 2
services            = nss, pam, ssh, sudo
domains             = default

[domain/default]

id_provider     = ldap
auth_provider   = ldap
chpass_provider = ldap
sudo_provider   = ldap

ldap_uri              = ldap://x.x.x.x/
ldap_search_base      = dc=example,dc=com
ldap_id_use_start_tls = False

ldap_search_timeout              = 3
ldap_network_timeout             = 3
ldap_opt_timeout                 = 3
ldap_enumeration_search_timeout  = 60
ldap_enumeration_refresh_timeout = 300
ldap_connection_expire_timeout   = 600

ldap_sudo_smart_refresh_interval = 600
ldap_sudo_full_refresh_interval  = 10800
# ldap_sudo_search_base = dc=sudoers,dc=example,dc=com

entry_cache_timeout = 1200
cache_credentials   = True

[nss]

homedir_substring = /home

entry_negative_timeout        = 20
entry_cache_nowait_percentage = 50

[pam]
reconnection_retries = 3
offline_credentials_expiration = 2
offline_failed_login_attempts = 3
offline_failed_login_delay = 5

[sudo]

[autofs]

[ssh]

[pac]

[ifp]
```

-  References
  - [None](https://kifarunix.com/how-to-configure-sudo-via-openldap-server/)
  - [Sudoers LDAP Manual](https://www.sudo.ws/man/1.8.17/sudoers.ldap.man.html)
  - [None](https://qrunch.net/@komu/entries/6k2cx7indCQYQAFy?ref=qrunch)

## ldap x freeradius
```
root@ldaptest:/etc/freeradius/3.0/mods-enabled# apt install freeradius-ldap
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following package was automatically installed and is no longer required:
  libdumbnet1
Use 'sudo apt autoremove' to remove it.
The following NEW packages will be installed:
  freeradius-ldap
0 upgraded, 1 newly installed, 0 to remove and 5 not upgraded.
Need to get 34.6 kB of archives.
After this operation, 135 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu bionic-updates/universe amd64 freeradius-ldap amd64 3.0.16+dfsg-1ubuntu3.1 [34.6 kB]
Fetched 34.6 kB in 1s (40.5 kB/s)
Selecting previously unselected package freeradius-ldap.
(Reading database ... 140817 files and directories currently installed.)
Preparing to unpack .../freeradius-ldap_3.0.16+dfsg-1ubuntu3.1_amd64.deb ...
Unpacking freeradius-ldap (3.0.16+dfsg-1ubuntu3.1) ...
Setting up freeradius-ldap (3.0.16+dfsg-1ubuntu3.1) ...

root@ldaptest:/etc/freeradius/3.0/mods-enabled# less /usr/lib/freeradius/rlm_l
rlm_ldap.so       rlm_linelog.so    rlm_logintime.so
```
- [ldap | FreeRADIUS Documentation](https://networkradius.com/doc/3.0.10/raddb/mods-available/ldap.html)

- この辺は使い方に応じて適切に．
```
# /etc/freeradius/3.0/mods-enable/ldap
        update {
                control:Password-With-Header    += 'userPassword'
#               control:NT-Password             := 'ntPassword'
#               reply:Reply-Message             := 'radiusReplyMessage'
#               reply:Tunnel-Type               := 'radiusTunnelType'
#               reply:Tunnel-Medium-Type        := 'radiusTunnelMediumType'
#               reply:Tunnel-Private-Group-ID   := 'radiusTunnelPrivategroupId'

                #  Where only a list is specified as the RADIUS attribute,
                #  the value of the LDAP attribute is parsed as a valuepair
                #  in the same format as the 'valuepair_attribute' (above).
                control:                        += 'radiusControlAttribute'
                request:                        += 'radiusRequestAttribute'
                reply:                          += 'radiusReplyAttribute'
        }
```

- ldapのposix groupをLDAP-Group paramと対応させたりできる．
```
# vim /etc/freeradius/3.0/mods-available/ldap
                base_dn = "${..base_dn}"

                #  Filter for group objects, should match all available
                #  group objects a user might be a member of.
                filter = '(objectClass=posixGroup)'

                # Search scope, may be 'base', 'one', sub' or 'children'
                scope = 'children'

                #  Attribute that uniquely identifies a group.
                #  Is used when converting group DNs to group
                #  names.
                name_attribute = cn

                #  Filter to find group objects a user is a member of.
                #  That is, group objects with attributes that
                #  identify members (the inverse of membership_attribute).
                membership_filter = "(|(member=%{control:Ldap-UserDn})(memberUid=%{%{Stripped-User-Name}:-%{User-Name}}))"
```

- そのLDAP-Groupをみてreplyを変えたりすることができる．
  - たとえば下記では `LDAP-Group == "users"` であれば replyに `cisco-avpair = "shell:priv-lvl=15"` を付与してciscoデバイスで特権モード操作が可能となる．
```
# /etc/freeradius/3.0/sites-enabled/default
post-auth{
        ldap
        if (LDAP-Group == "users") {
                update reply {
                        cisco-avpair = "shell:priv-lvl=15"
                }
        #}elsif (LDAP-Group == "ou=users,dc=example,dc=com") {
        #       update reply {
        #               cisco-avpair = "shell:priv-lvl=15"
        #       }
        }
```

## todo
- group nameをsss client側/ldap client側で出したい場合どうすればいいの．gidはclientでもててるがnameにmapできてない．

## References
- [freeradius/openldap.schema at master · redBorder/freeradius · GitHub](https://github.com/redBorder/freeradius/blob/master/doc/schemas/ldap/openldap.schema)
- [git.samba.org - samba.git/blob - examples/LDAP/samba.schema](https://git.samba.org/?p=samba.git;a=blob;f=examples/LDAP/samba.schema)
- [GitHub - AndriiGrytsenko/openssh-ldap-publickey: Wrapper for OpenSSH to store public keys inside the OpenLDAP entry.](https://github.com/AndriiGrytsenko/openssh-ldap-publickey)
- [None](https://www.osstech.co.jp/~hamano/posts/centos7-openldap-ssh/openssh-lpk-openldap.ldif)
- [Ubuntu 16.04上でオープンLDAPのインストールと設定 - hirosanote’s blog](http://hirosanote.hatenablog.jp/entry/2018/01/27/221527)
- [None](https://kifarunix.com/configure-sssd-for-openldap-authentication-on-ubuntu-18-04/)
- [Ubuntu 14.04 SSSD and OpenLDAP Authentication](https://www.ossramblings.com/Ubuntu-14.04-SSSD-and-OpenLDAP-Authentication)
- [LDAPで構造型オブジェクトクラスは変更できない？ | OpenGroove](https://open-groove.net/openldap/change-structual-object-class/)
- [LDAPにユーザ、グループを追加し、Linuxアカウントとして設定したときの備忘録 - tail -f /var/log/こうちかずお.log](https://kohchi.hatenablog.com/entry/20131201/1385885485)
- [Sudoers LDAP Manual](https://www.sudo.ws/man/1.8.17/sudoers.ldap.man.html#EXAMPLES)
- [None](https://www.adimian.com/blog/2014/10/how-to-enable-memberof-using-openldap/)
- [FreeRADIUS Post Authentication – Bliss](https://jbliss.net/2019/04/freeradius-post-authentication/)
- [modules/Rlm_ldap](https://wiki.freeradius.org/modules/Rlm_ldap)