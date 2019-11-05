# LDAP

## ldapにのっけておくべきなpractice
```
cn=yudai.hashimoto
dn: uid=yudai.hashimoto,ou=people,dc=<domain1>,dc=<domain0>
//ouでserverとかの機器，peopleとかを分けておくと良さそう．dcはdomainがちょうどいいかも．
uid: yudai.hashimoto
objectClass: userAccount
objectClass: radiusprofile
objectClass: posixAccount
cn: yudai.hashimoto
sn: Yudai Hashimoto
uidNumber: ****
gidNumber: ****
homeDirectory: /home/yudai.hashimoto
gecos: Yudai Hashimoto
loginShell: /usr/local/bin/bash
publickey: ****
mail: yudai.hashimoto@jp7fkf.dev
mailRecipient: yudai.hashimoto@jp7fkf.dev
//その他付加的な属性をお好みでスキーマ定義してつけるとか．
```