# VPP
## VPP install battle (CentOS)

- https://packagecloud.io/fdio/master/install#bash-rpm
- [VPP/Setting Up Your Dev Environment - fd.io](https://wiki.fd.io/view/VPP/Setting_Up_Your_Dev_Environment)

- とりあえずパッケージをいれる．
```
yum update -y
curl -s https://packagecloud.io/install/repositories/fdio/master/script.rpm.sh | sudo bash
yum install -y vpp
yum install -y vpp-plugins vpp-devel

[root@localhost ~]# systemctl start vpp
[root@localhost ~]#
[root@localhost ~]# systemctl status vpp
● vpp.service - Vector Packet Processing Process
   Loaded: loaded (/usr/lib/systemd/system/vpp.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2018-12-10 13:07:46 JST; 2s ago
  Process: 25261 ExecStartPre=/sbin/modprobe uio_pci_generic (code=exited, status=0/SUCCESS)
  Process: 25260 ExecStartPre=/bin/rm -f /dev/shm/db /dev/shm/global_vm /dev/shm/vpe-api (code=exited, status=0/SUCCESS)
 Main PID: 25266 (vpp_main)
   CGroup: /system.slice/vpp.service
           └─25266 /usr/bin/vpp -c /etc/vpp/startup.conf

Dec 10 13:07:46 localhost.localdomain vpp[25266]: /usr/bin/vpp[25266]: vlib_pci_bind_to_...up
Dec 10 13:07:46 localhost.localdomain vpp[25266]: /usr/bin/vpp[25266]: vlib_pci_bind_to_...up
Dec 10 13:07:46 localhost.localdomain vpp[25266]: /usr/bin/vpp[25266]: vlib_pci_bind_to_...up
Dec 10 13:07:46 localhost.localdomain vpp[25266]: /usr/bin/vpp[25266]: vlib_pci_bind_to_...up
Dec 10 13:07:46 localhost.localdomain vpp[25266]: /usr/bin/vpp[25266]: dpdk: EAL init ar...64
Dec 10 13:07:46 localhost.localdomain /usr/bin/vpp[25266]: vlib_pci_bind_to_uio: Skipping...p
Dec 10 13:07:46 localhost.localdomain /usr/bin/vpp[25266]: vlib_pci_bind_to_uio: Skipping...p
Dec 10 13:07:46 localhost.localdomain /usr/bin/vpp[25266]: vlib_pci_bind_to_uio: Skipping...p
Dec 10 13:07:46 localhost.localdomain /usr/bin/vpp[25266]: dpdk: EAL init args: -c 1 -n 4...4
Dec 10 13:07:47 localhost.localdomain vnet[25266]: dpdk_ipsec_process:1010: not enough D...SL
Hint: Some lines were ellipsized, use -l to show in full.

```

- 見えて来ない
```
[root@localhost ~]# vppctl sh int
              Name               Idx    State  MTU (L3/IP4/IP6/MPLS)     Counter          Count
local0                            0     down          0/0/0/0
```

- pci-id指定して挿す．
```
[root@localhost ~]# lspci
## 刺したいifのpci-idを特定する．
[root@localhost ~]# vi /etc/vpp/startup.conf
## dpdk の　comment out をはずして，
## dev <pci-id>的に書く．

[root@localhost ~]# ifconfig | grep ens

## vppにつなぎたいif を調べる．
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
ens224: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
ens256: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500

[root@localhost ~]# ifconfig ens192 down
[root@localhost ~]# ifconfig ens224 down

[root@localhost ~]# ip addr flush dev ens192
[root@localhost ~]# ip addr flush dev ens224
[root@localhost ~]# systemctl stop vpp
[root@localhost ~]# systemctl start vpp
[root@localhost ~]# systemctl status vpp
● vpp.service - Vector Packet Processing Process
   Loaded: loaded (/usr/lib/systemd/system/vpp.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2018-12-10 14:36:18 JST; 3s ago
  Process: 22985 ExecStartPre=/sbin/modprobe uio_pci_generic (code=exited, status=0/SUCCESS)
  Process: 22984 ExecStartPre=/bin/rm -f /dev/shm/db /dev/shm/global_vm /dev/shm/vpe-api (code=exited, status=0/SUCCESS)
 Main PID: 22987 (vpp_main)
   CGroup: /system.slice/vpp.service
           └─22987 /usr/bin/vpp -c /etc/vpp/startup.conf

Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: memif_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: nat_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: nsh_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: nsim_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: pppoe_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: stn_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: load_one_vat_plugin:67: Loaded plugin: vmxnet3_test_plugin.so
Dec 10 14:36:18 localhost.localdomain /usr/bin/vpp[22987]: dpdk: EAL init args: -c 1 -n 4 --huge-dir /run/vpp/hugepages --file-prefix vpp -w 0000:0b:00.0 -w 0000:13...et-mem 64
Dec 10 14:36:18 localhost.localdomain vpp[22987]: /usr/bin/vpp[22987]: dpdk: EAL init args: -c 1 -n 4 --huge-dir /run/vpp/hugepages --file-prefix vpp -w 0000:0b:00...ket-mem 64
Dec 10 14:36:19 localhost.localdomain vnet[22987]: dpdk_ipsec_process:1010: not enough DPDK crypto resources, default to OpenSSL
Hint: Some lines were ellipsized, use -l to show in full.

[root@localhost ~]# vppctl sh int
              Name               Idx    State  MTU (L3/IP4/IP6/MPLS)     Counter          Count
GigabitEthernet13/0/0             2     down         9000/0/0/0
GigabitEthernetb/0/0              1     down         9000/0/0/0
local0                            0     down          0/0/0/0
```
- 挿さった．

## SRv6 してみる
- [VPP/Segment Routing for IPv6 - fd.io](https://wiki.fd.io/view/VPP/Segment_Routing_for_IPv6:_Overlay_VPN_with_Underlay_Optimization)

```
vpp# sh int
              Name               Idx    State  MTU (L3/IP4/IP6/MPLS)     Counter          Count
GigabitEthernet13/0/0             2     down         9000/0/0/0
GigabitEthernetb/0/0              1     down         9000/0/0/0
local0                            0     down          0/0/0/0
vpp# sh int addr
GigabitEthernet13/0/0 (dn):
GigabitEthernetb/0/0 (dn):
local0 (dn):
vpp#
vpp# set int ip address GigabitEthernet13/0/0 10.0.2.20/24
vpp# set int ip address GigabitEthernetb/0/0 10.0.4.10/24
vpp#
vpp# set int state GigabitEthernet13/0/0 up
vpp# set int state GigabitEthernetb/0/0 up
vpp# sh int
              Name               Idx    State  MTU (L3/IP4/IP6/MPLS)     Counter          Count
GigabitEthernet13/0/0             2      up          9000/0/0/0     rx packets                     2
                                                                    rx bytes                     120
                                                                    drops                          2
                                                                    ip4                            2
GigabitEthernetb/0/0              1      up          9000/0/0/0     rx packets                     2
                                                                    rx bytes                     120
                                                                    drops                          2
                                                                    ip4                            2
local0                            0     down          0/0/0/0
vpp# sh int addr
GigabitEthernet13/0/0 (up):
  L3 10.0.2.20/24
GigabitEthernetb/0/0 (up):
  L3 10.0.4.10/24
local0 (dn):
vpp#
vpp# set sr encaps source addr C1::
vpp# sr policy add bsid C1::999:1 next C2:: next C4::6 encap
vpp# sr steer l3 B::/112 via sr policy bsid C1::999:1
sr steer: No SR policy specified
```