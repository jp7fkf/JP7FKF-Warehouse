# Raspberry Pi + GPS ModuleでStratum1なNTPサーバをつくる

## 配線
- あとでかくぞ．

## 大切なこと
- raspberry piのserial consoleをdisableすること．
- serialがttyAMA0に割り当てられること
- gpsmon/cgpsを利用してppsのoffsetを掌握し，/etc/chrony/chrony.confのoffsetを調整すること．
  - これをやらないと処理遅延によるoffsetがかかってしまって絶対時間が合わない．
  - いくらgpsのppsを利用しているとはいえ，offsetは避けられない．設定しない場合でもあるoffsetをもつjitter/err marginの少ないNTPとしては成立すると思う．
  - `refclock SHM 1 lock NMEA refid PPS precision 1e-9 offset 0.100500`

## procedure
```
jp7fkf@lab1:~ $ uname -a
Linux rasp03r.live 4.19.97+ #1294 Thu Jan 30 13:10:54 GMT 2020 armv6l GNU/Linux
jp7fkf@lab1:~ $ sudo systemctl stop serial-getty@ttyAMA0.service
jp7fkf@lab1:~ $ sudo systemctl disable serial-getty@ttyAMA0.service
jp7fkf@lab1:~ $ sudo apt install gpsd gpsd-clients pps-tools
jp7fkf@lab1:~ $
jp7fkf@lab1:~ $
jp7fkf@lab1:~ $ sudo timedatectl status
               Local time: Fri 2020-02-28 19:14:50 JST
           Universal time: Fri 2020-02-28 10:14:50 UTC
                 RTC time: n/a
                Time zone: Asia/Tokyo (JST, +0900)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
jp7fkf@lab1:~ $
jp7fkf@lab1:~ $ cat /boot/cmdline.txt
console=tty1 root=PARTUUID=738a4d67-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait
jp7fkf@lab1:~ $ cat /boot/config.txt
# For more options and information see
# http://rpf.io/configtxt
# Some settings may impact device functionality. See link above for details

# uncomment if you get no picture on HDMI for a default "safe" mode
#hdmi_safe=1

# uncomment this if your display has a black border of unused pixels visible
# and your display can output without overscan
#disable_overscan=1

# uncomment the following to adjust overscan. Use positive numbers if console
# goes off screen, and negative if there is too much border
#overscan_left=16
#overscan_right=16
#overscan_top=16
#overscan_bottom=16

# uncomment to force a console size. By default it will be display's size minus
# overscan.
#framebuffer_width=1280
#framebuffer_height=720

# uncomment if hdmi display is not detected and composite is being output
#hdmi_force_hotplug=1

# uncomment to force a specific HDMI mode (this will force VGA)
#hdmi_group=1
#hdmi_mode=1

# uncomment to force a HDMI mode rather than DVI. This can make audio work in
# DMT (computer monitor) modes
#hdmi_drive=2

# uncomment to increase signal to HDMI, if you have interference, blanking, or
# no display
#config_hdmi_boost=4

# uncomment for composite PAL
#sdtv_mode=2

#uncomment to overclock the arm. 700 MHz is the default.
#arm_freq=800

# Uncomment some or all of these to enable the optional hardware interfaces
#dtparam=i2c_arm=on
#dtparam=i2s=on
#dtparam=spi=on

# Uncomment this to enable infrared communication.
#dtoverlay=gpio-ir,gpio_pin=17
#dtoverlay=gpio-ir-tx,gpio_pin=18

# Additional overlays and parameters are documented /boot/overlays/README

# Enable audio (loads snd_bcm2835)
dtparam=audio=on

[pi4]
# Enable DRM VC4 V3D driver on top of the dispmanx display stack
dtoverlay=vc4-fkms-v3d
max_framebuffers=2

[all]
#dtoverlay=vc4-fkms-v3d
enable_uart=1

core_freq=250                        #added!
dtoverlay=pps-gpio,gpiopin=18        #added!
jp7fkf@lab1:~ $ cat /etc/modules
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
pps-gpio/etc/default/gpsd
jp7fkf@lab1:~ $ cat
# Default settings for the gpsd init script and the hotplug wrapper.

# Start the gpsd daemon automatically at boot time
START_DAEMON="true"

# Use USB hotplugging to add new USB devices automatically to the daemon
USBAUTO="true"

# Devices gpsd should collect to at boot time.
# They need to be read/writeable, either by user gpsd or the group dialout.
DEVICES="/dev/ttyAMA0 /dev/pps0"

# Other options you want to pass to gpsd
GPSD_OPTIONS="-n"
jp7fkf@lab1:~ $ cgps -s
jp7fkf@lab1:~ $ gpsmon
jp7fkf@lab1:~ $ sudo systemctl disable systemd-timesyncd
Removed /etc/systemd/system/sysinit.target.wants/systemd-timesyncd.service.
Removed /etc/systemd/system/dbus-org.freedesktop.timesync1.service.
jp7fkf@lab1:~ $ sudo systemctl stop systemd-timesyncd
jp7fkf@lab1:~ $
jp7fkf@lab1:~ $ sudo apt install chrony
# Welcome to the chrony configuration file. See chrony.conf(5) for more
# information about usuable directives.
# pool 2.debian.pool.ntp.org iburst
pool ntp.nict.jp maxsources 4 noselect
pool ntp.jst.mfeed.ad.jp maxsources 4 noselect

# NEMA by GPS
refclock SHM 0 refid NMEA precision 1e-1 noselect
refclock SHM 1 lock NMEA refid PPS precision 1e-9 offset 0.100500

# This directive specify the location of the file containing ID/key pairs for
# NTP authentication.
keyfile /etc/chrony/chrony.keys

# This directive specify the file into which chronyd will store the rate
# information.
driftfile /var/lib/chrony/chrony.drift

# Uncomment the following line to turn logging on.
#log tracking measurements statistics

# Log files location.
logdir /var/log/chrony

# Stop bad estimates upsetting machine clock.
maxupdateskew 100.0

# This directive enables kernel synchronisation (every 11 minutes) of the
# real-time clock. Note that it can’t be used along with the 'rtcfile' directive.
rtcsync

# Step the system clock instead of slewing it if the adjustment is larger than
# one second, but only in the first three clock updates.
makestep 1 3

# Allow NTP client access from local network.
allow 10.1.0.0/24
allow 10.2.0.0/24
jp7fkf@lab1:~ $ chronyc sources -v
210 Number of sources = 2

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current synced, '+' = combined , '-' = not combined,
| /   '?' = unreachable, 'x' = time may be in error, '~' = time too variable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
#? NMEA                          0   4   377    23   +301ms[ +301ms] +/-  100ms
#* PPS                           0   4   377    21  +1232ns[+1929ns] +/- 1000ns
```

## memo
- `ppstest /dev/pps0`

## References
- [GPSD Time Service HOWTO](https://gpsd.gitlab.io/gpsd/gpsd-time-service-howto.html)
- [Raspberry Pi 3 Model B + USB接続GPSレシーバ + ChronyでStratum 1なNTPサーバを作る – Lunatilia](https://lunatilia.wordpress.com/2019/01/29/building_stratum-1_ntp-server_with_raspi3_and_bu-353s4_and_chrony/)
- [GPSモジュールを接続したRaspberry piでchronyを動作 - Qiita](https://qiita.com/RCA3610/items/0b3671cd272e430083d1)
- [Raspberry pi3とGPSモジュールを接続してNTPサーバを構築 - Qiita](https://qiita.com/RCA3610/items/c478bb087acdad418cd3)
- [GPS電波をRaspberry Piで読み取り、時間と位置把握する](https://www.rs-online.com/designspark/add-gps-time-and-location-to-a-raspberry-pi-project-jp)
- [AE-GNSS_manual_r1.00s.pdf](http://akizukidenshi.com/download/ds/akizuki/AE-GNSS_manual_r1.00s.pdf)
- [17.3. chrony の使用 Red Hat Enterprise Linux 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-using_chrony)