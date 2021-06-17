# Allied-Telesis

## X510 bootloader versionup battle
```
awplus#show system
Stack System Status                                   Thu Jun 17 02:35:41 2021

Stack member 1

Board       ID  Bay   Board Name                         Rev   Serial number
--------------------------------------------------------------------------------
Base       369        x510-28GTX                         B-0   XXXXXXXXX
--------------------------------------------------------------------------------
RAM:  Total: 494844 kB Free: 375548 kB
Flash: 63.0MB Used: 25.3MB Available: 37.7MB
--------------------------------------------------------------------------------
Environment Status : ***Fault***
Uptime             : 0 days 00:31:12
Bootloader version : 2.0.14


Current software   : x510-5.4.4-3.10.rel
Software version   : 5.4.4-3.10
Build date         : Mon Jul 6 07:25:52 UTC 2015

Current boot config: flash:/default.cfg (file exists)

System Name
 awplus
System Contact

System Location

```
> Bootloader version : 2.0.14
なので `x510-5.4.8-2.9a.rel` をあてる．

空き容量チェック
```
awplus#show file systems

 Size(b)  Free(b)  Type   Flags  Prefixes   S/D/V   Lcl/Ntwk  Avail
-------------------------------------------------------------------
  63.0M    32.0M   flash     rw  flash:     static  local      Y
      -        -   system    rw  system:    virtual local      -
  10.0M     9.9M   debug     rw  debug:     static  local      Y
 499.0K   464.0K   nvs       rw  nvs:       static  local      Y
      -        -   usbstick  rw  usb:       dynamic local      N
      -        -   fserver   rw  fserver:   dynamic network    N
      -        -   tftp      rw  tftp:      -       network    -
      -        -   scp       rw  scp:       -       network    -
      -        -   sftp      ro  sftp:      -       network    -
      -        -   http      ro  http:      -       network    -
      -        -   rsync     rw  rsync:     -       network    -
```

転送
```
% scp x510-5.4.8-2.9a.rel manager@10.0.0.1:x510-5.4.8-2.9a.rel
Password:
x510-5.4.8-2.9a.rel                                    100%   25MB 389.0KB/s   01:06
```

```
awplus#show file sys

 Size(b)  Free(b)  Type   Flags  Prefixes   S/D/V   Lcl/Ntwk  Avail
-------------------------------------------------------------------
  63.0M    12.0M   flash     rw  flash:     static  local      Y
      -        -   system    rw  system:    virtual local      -
  10.0M     9.9M   debug     rw  debug:     static  local      Y
 499.0K   464.0K   nvs       rw  nvs:       static  local      Y
      -        -   usbstick  rw  usb:       dynamic local      N
      -        -   fserver   rw  fserver:   dynamic network    N
      -        -   tftp      rw  tftp:      -       network    -
      -        -   scp       rw  scp:       -       network    -
      -        -   sftp      ro  sftp:      -       network    -
      -        -   http      ro  http:      -       network    -
      -        -   rsync     rw  rsync:     -       network    -

awplus#dir
 26549894 -rw- Jun 17 2021 02:51:23  x510-5.4.8-2.9a.rel
      861 -rw- Jun 17 2021 02:37:41  default.cfg
 24319922 -rw- Nov 19 2015 08:53:23  x510-5.4.4-3.10.rel
```
転送ok

verup
```
awplus#show boot
Boot configuration
--------------------------------------------------------------------------------
Current software   : x510-5.4.4-3.10.rel
Current boot image : flash:/x510-5.4.4-3.10.rel (file exists)
Backup  boot image : Not set
Default boot config: flash:/default.cfg
Current boot config: flash:/default.cfg (file exists)
Backup  boot config: Not set
Autoboot status    : disabled

awplus#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
awplus(config)#boot system x510-5.4.8-2.9a.rel
awplus(config)#boot system backup x510-5.4.4-3.10.rel
awplus(config)#end
awplus#
awplus#show boot
Boot configuration
--------------------------------------------------------------------------------
Current software   : x510-5.4.4-3.10.rel
Current boot image : flash:/x510-5.4.8-2.9a.rel (file exists)
Backup  boot image : flash:/x510-5.4.4-3.10.rel (file exists)
Default boot config: flash:/default.cfg
Current boot config: flash:/default.cfg (file exists)
Backup  boot config: Not set
Autoboot status    : disabled
awplus#reboot
reboot system? (y/n): y
```

起動シーケンス
```
Machine Restart
�
Bootloader 2.0.14 loaded
Press <Ctrl+B> for the Boot Menu

Reading filesystem...
Loading flash:x510-5.4.8-2.9a.rel...
Verifying release... OK
Booting...
Starting base/first...                                  [  OK  ]
Mounting virtual filesystems...                         [  OK  ]

           ______________   ____
        /\ \            / /______\
      /   \ \_      __/ /| ______ |
    /      \ |     |  /  | ______ |
  /         \ \   / /     \ ____ /
/______/\____\ \/ /____________/

Allied Telesis Inc.
AlliedWare Plus (TM) v5.4.8
Current release filename: x510-5.4.8-2.9a.rel
Built: Wed Jun 19 12:08:33 UTC 2019
Mounting static filesystems...                          [  OK  ]
Checking flash filesystem...                            [  OK  ]
Mounting flash filesystem...                            [  OK  ]
Checking NVS filesystem...                              [  OK  ]
Mounting NVS filesystem...                              [  OK  ]
Verifying bootloader...                                 [FAILED]
 +----------------------------------------------------+    |
 | Expected bootloader version 2.0.36, found 2.0.14   |----+
 +----------------------------------------------------+
Updating bootloader...                                  [  OK  ]
cat: can't open '/var/lib/stack/is_stacked': No such file or directory
sh: 1: unknown operand
fastboot: rebooted by ghost: Special firmware

Flushing file system buffers...reboot: Restarting system

Unmounting any remaining filesystems...
Machine Restart
Please stand by while rebooting the system...
�
Bootloader 2.0.36 loaded
Press <Ctrl+B> for the Boot Menu

Reading filesystem...
Loading flash:x510-5.4.4-3.10.rel...
Verifying release... OK
Booting...
Starting base/first...                                  [  OK  ]
Mounting virtual filesystems...                         [  OK  ]

           ______________   ____
        /\ \            / /______\
      /   \ \_      __/ /| ______ |
    /      \ |     |  /  | ______ |
  /         \ \   / /     \ ____ /
/______/\____\ \/ /____________/

Allied Telesis Inc.
AlliedWare Plus (TM) v5.4.4
Current release filename:x510-5.4.4-3.10.rel
Original release filename: x510-5.4.4-3.10.rel
Built: Mon Jul 6 07:25:51 UTC 2015
Mounting static filesystems...                          [  OK  ]
Checking flash filesystem...                            [  OK  ]
...
```
bootloaderがverupされた．

```
awplus#show system  | inc Boot
Bootloader version : 2.0.36
awplus#show boot
Boot configuration
--------------------------------------------------------------------------------
Current software   : x510-5.4.4-3.10.rel
Current boot image : flash:/x510-5.4.4-3.10.rel (file exists)
Backup  boot image : Not set
Default boot config: flash:/default.cfg
Current boot config: flash:/default.cfg (file exists)
Backup  boot config: Not set
Autoboot status    : disabled
Boot Security Level: none

awplus#dir
      861 -rw- Jun 17 2021 02:55:45  default.cfg
 26549894 -rw- Jun 17 2021 02:51:23  x510-5.4.8-2.9a.rel
 24319922 -rw- Nov 19 2015 08:53:23  x510-5.4.4-3.10.rel
```
このfirmはboot firmupのためのものなので通常の起動で使うべきfirmではない．したがって勝手にbackupのものがprimary imageになるようになっている．

## つづいてfirm verup battle
```
awplus#show system
Stack System Status                                   Thu Jun 17 03:07:38 2021

Stack member 1

Board       ID  Bay   Board Name                         Rev   Serial number
--------------------------------------------------------------------------------
Base       369        x510-28GTX                         B-0   XXXXXXXXX
--------------------------------------------------------------------------------
RAM:  Total: 494844 kB Free: 373972 kB
Flash: 63.0MB Used: 35.6MB Available: 27.4MB
--------------------------------------------------------------------------------
Environment Status : ***Fault***
Uptime             : 0 days 00:10:19
Bootloader version : 2.0.36


Current software   : x510-5.4.4-3.10.rel
Software version   : 5.4.4-3.10
Build date         : Mon Jul 6 07:25:52 UTC 2015

Current boot config: flash:/default.cfg (file exists)

System Name
 awplus
System Contact

System Location

awplus#show file system

 Size(b)  Free(b)  Type   Flags  Prefixes   S/D/V   Lcl/Ntwk  Avail
-------------------------------------------------------------------
  63.0M    37.7M   flash     rw  flash:     static  local      Y
      -        -   system    rw  system:    virtual local      -
  10.0M     9.9M   debug     rw  debug:     static  local      Y
 499.0K   464.0K   nvs       rw  nvs:       static  local      Y
      -        -   usbstick  rw  usb:       dynamic local      N
      -        -   fserver   rw  fserver:   dynamic network    N
      -        -   tftp      rw  tftp:      -       network    -
      -        -   scp       rw  scp:       -       network    -
      -        -   sftp      ro  sftp:      -       network    -
      -        -   http      ro  http:      -       network    -
      -        -   rsync     rw  rsync:     -       network    -

awplus#dir
      861 -rw- Jun 17 2021 02:55:45  default.cfg
 24319922 -rw- Nov 19 2015 08:53:23  x510-5.4.4-3.10.rel
```

firm転送
```
% scp x510-5.5.1-0.1.rel manager@10.0.0.1:x510-5.5.1-0.1.rel
Password:
x510-5.5.1-0.1.rel                                     100%   25MB 564.5KB/s   00:46
```

```
awplus#dir
 26639992 -rw- Jun 17 2021 16:08:05  x510-5.5.1-0.1.rel
      861 -rw- Jun 17 2021 02:55:45  default.cfg
 24319922 -rw- Nov 19 2015 08:53:23  x510-5.4.4-3.10.rel
awplus(config)#do show boot
Boot configuration
--------------------------------------------------------------------------------
Current software   : x510-5.4.4-3.10.rel
Current boot image : flash:/x510-5.4.4-3.10.rel (file exists)
Backup  boot image : Not set
Default boot config: flash:/default.cfg
Current boot config: flash:/default.cfg (file exists)
Backup  boot config: Not set
Autoboot status    : disabled
Boot Security Level: none

awplus(config)#boot system x510-5.5.1-0.1.rel
awplus(config)#boot system backup x510-5.4.4-3.10.rel
awplus(config)#do show boot
Boot configuration
--------------------------------------------------------------------------------
Current software   : x510-5.4.4-3.10.rel
Current boot image : flash:/x510-5.5.1-0.1.rel (file exists)
Backup  boot image : flash:/x510-5.4.4-3.10.rel (file exists)
Default boot config: flash:/default.cfg
Current boot config: flash:/default.cfg (file exists)
Backup  boot config: Not set
Autoboot status    : disabled
Boot Security Level: none

awplus(config)# exit
awplus#reboot
reboot system? (y/n): y
```

あがってくる
```
Machine Restart
�
Bootloader 2.0.36 loaded
Press <Ctrl+B> for the Boot Menu

Reading filesystem...
Loading flash:x510-5.5.1-0.1.rel...
Verifying release... OK
Booting...
Linux version 5.7.19-at1 (maker@maker07-build) (gcc version 8.3.0 (crosstool-NG 1.24.0), GNU ld (crosstool-NG 1.24.0) 2.32) #1 Mon Apr 12 02:18:23 UTC 2021
printk: bootconsole [early0] enabled
CPU0 revision is: 00019749 (MIPS 74Kc)
MIPS: machine is Allied Telesis x510/IX5/IE510/SH510
Starting base/first...                                  [  OK  ]
Mounting virtual filesystems...                         [  OK  ]

           ______________   ____
        /\ \            / /______\
      /   \ \_      __/ /| ______ |
    /      \ |     |  /  | ______ |
  /         \ \   / /     \ ____ /
/______/\____\ \/ /____________/

Allied Telesis Inc.
AlliedWare Plus (TM) v5.5.1
Current release filename: x510-5.5.1-0.1.rel
Built: Mon Apr 12 03:01:37 UTC 2021
Mounting static filesystems...                          [  OK  ]
Checking flash filesystem...                            [  OK  ]
Mounting flash filesystem...                            [  OK  ]
Checking NVS filesystem...                              [  OK  ]
Mounting NVS filesystem...                              [  OK  ]
...
```

## fan静音化
- 'edit fan.sh' して書きを記述．C-k,s でsave, C-cでexit．
```
#!/bin/sh

# 低回転エラー避け
echo 1000 > /sys/class/hwmon/hwmon0/fan1_min

# 回転速度を0-255で指定
echo 64 > /sys/class/hwmon/hwmon0/pwm1_auto_point1_pwm
echo 64 > /sys/class/hwmon/hwmon0/pwm1_auto_point2_pwm

# 以下、メモ
#cat /sys/class/hwmon/hwmon0/device/pwm1_auto_channels_temp
#cat /sys/class/hwmon/hwmon0/device/pwm1_auto_point1_pwm
#cat /sys/class/hwmon/hwmon0/device/pwm1_auto_point2_pwm
#cat /sys/class/hwmon/hwmon0/device/pwm1_enable
#cat /sys/class/hwmon/hwmon0/device/pwm1_freq
#cat /sys/class/hwmon/hwmon0/device/pwm1
```
シェルスクリプトを走らせる
```
awplus#activate fan.sh
awplus#show sys env
Environment Monitoring Status

Overall Status: ***Fault***

Resource ID: 1  Name: RPS 1 ()
ID  Sensor (Units)                      Reading  Low Limit High Limit Status
1   Device Present                          Yes        -          -       Ok
2   PSU Power Output                        Yes        -          -       Ok

Resource ID: 2  Name: RPS 2 ()
ID  Sensor (Units)                      Reading  Low Limit High Limit Status
1   Device Present                          Yes        -          -       Ok
2   PSU Power Output                         No        -          -    FAULT

Resource ID: 3  Name: x510-28GTX
ID  Sensor (Units)                      Reading  Low Limit High Limit Status
1   Fan: Fan 1 (Rpm)                       1911       1000        -       Ok
2   Voltage: 1.8V (Volts)                 1.813      1.612      1.975     Ok
3   Voltage: 1.0V (Volts)                 0.999      0.891      1.090     Ok
4   Voltage: 3.3V (Volts)                 3.278      3.028      3.562     Ok
5   Voltage: 5.0V (Volts)                 5.066      4.503      5.498     Ok
6   Voltage: 1.2V (Volts)                 1.184      1.046      1.292     Ok
7   Temp: CPU (Degrees C)                    49        -11         80     Ok
```
