# Strongswan

## ubuntuでstrongswanしてlogを設定するとapparmorに怒られた話

例えばこんな風にして軽率に `/var/log` 配下にlogを書こうとする
```
jp7fkf@lab1:~$ cat /etc/strongswan.d/charon-logging.conf
charon {
    filelog {
        default = 1
        /var/log/charon.log {
            path = /var/log/charon.log
            time_format = %b %e %T
            ike_name = yes
            append = no
            default = 1
            flush_line = yes
            time_add_ms = yes
            net = 2
            tls = 2
            knl = 2
        }
    }
}
```

しかしlogは `/var/log/charon.log` には出てこない．
syslogを覗いてみるとこんなmessageが．

```
Feb 27 16:17:48 lab1 kernel: [ 8212.229038] audit: type=1400 audit(1582820000.905:38): apparmor="DENIED" operation="mknod" profile="/usr/lib/ipsec/charon" name="/var/log/charon.log" pid=16202 comm="charon" requested_mask="c" denied_mask="c" fsuid=0 ouid=0
```
> apparmor="DENIED"
と言われているのでapparmorで `/var/log/charon.log` への書き込みが弾かれてるっぽい．

適切に権限をあげればいい気がする．
apparmorに`/var/log/charon.log*`への書き込み権限を加える
`/etc/apparmor.d/` 配下でcharonのapparmor settingを見ていじる．

```
jp7fkf@lab1:~$ sudo cat /etc/apparmor.d/usr.lib.ipsec.charon
# ------------------------------------------------------------------
#
#   Copyright (C) 2016 Canonical Ltd.
#
#   This program is free software; you can redistribute it and/or
#   modify it under the terms of version 2 of the GNU General Public
#   License published by the Free Software Foundation.
#
#   Author: Jonathan Davies <jonathan.davies@canonical.com>
#           Ryan Harper <ryan.harper@canonical.com>
#
# ------------------------------------------------------------------

#include <tunables/global>

/usr/lib/ipsec/charon flags=(attach_disconnected) {
  #include <abstractions/base>
  #include <abstractions/nameservice>
  #include <abstractions/authentication>
  #include <abstractions/openssl>
  #include <abstractions/p11-kit>

  capability ipc_lock,
  capability net_admin,
  capability net_raw,

  # allow priv dropping (LP: #1333655)
  capability chown,
  capability setgid,
  capability setuid,

  # libcharon-extra-plugins: xauth-pam
  capability audit_write,

  # libstrongswan-standard-plugins: agent
  capability dac_override,

  capability net_admin,
  capability net_raw,

  network,
  network raw,

  /bin/dash                 rmPUx,

  # libchron-extra-plugins: kernel-libipsec
  /dev/net/tun              rw,

  /etc/ipsec.conf           r,
  /etc/ipsec.secrets        r,
  /etc/ipsec.*.secrets      r,
  /etc/ipsec.d/             r,
  /etc/ipsec.d/**           r,
  /etc/ipsec.d/crls/*       rw,
  /etc/opensc/opensc.conf   r,
  /etc/strongswan.conf      r,
  /etc/strongswan.d/        r,
  /etc/strongswan.d/**      r,
  /etc/tnc_config           r,

  /proc/sys/net/core/xfrm_acq_expires   w,

  /run/charon.*             rw,
  /run/pcscd/pcscd.comm     rw,

  /usr/lib/ipsec/charon     rmix,
  /usr/lib/ipsec/imcvs/     r,
  /usr/lib/ipsec/imcvs/**   rm,

  /usr/lib/*/opensc-pkcs11.so rm,

  /var/lib/strongswan/*     r,

  /var/log/charon.log*       rw, # ここに/var/log/charon.log*のrw権限をつける．

  # for using the ha plugin (LP: #1773956)
  @{PROC}/@{pid}/net/ipt_CLUSTERIP/ r,
  @{PROC}/@{pid}/net/ipt_CLUSTERIP/* rw,

  # Site-specific additions and overrides. See local/README for details.
  #include <local/usr.lib.ipsec.charon>
}

```
あとはapparmorをreloadさせる．
`sudo systemctl reload apparmor.service`

するとlogがちゃんと出てくる．
```
jp7fkf@lab1:~$ cat /var/log/charon.log
Feb 27 16:22:44.144 00[DMN] Starting IKE charon daemon (strongSwan 5.6.2, Linux 5.0.0-1031-gcp, x86_64)
Feb 27 16:22:44.454 00[KNL] known interfaces and IP addresses:
Feb 27 16:22:44.454 00[KNL]   lo
Feb 27 16:22:44.454 00[KNL]     127.0.0.1
Feb 27 16:22:44.454 00[KNL]     ::1
(omit...)
```

めでたし．