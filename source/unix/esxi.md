# ESXi


## 6.0 -> 6.5 update log

```
% ssh 172.16.30.35 -l root
Password:
The time and date of this login have been sent to the system logs.

VMware offers supported, powerful system administration tools.  Please
see www.vmware.com/go/sysadmintools for details.

The ESXi Shell can be disabled by an administrative user. See the
vSphere Security documentation for more information.
[root@localhost:~]
[root@localhost:~] ls
altbootbank      dev              locker           productLocker    tardisks         var
bin              etc              mbr              sbin             tardisks.noauto  vmfs
bootbank         lib              opt              scratch          tmp              vmimages
bootpart.gz      lib64            proc             store            usr              vmupgrade
[root@localhost:~]
[root@localhost:~] esxcli system version get
   Product: VMware ESXi
   Version: 6.0.0
   Build: Releasebuild-3073146
   Update: 1
   Patch: 20
[root@localhost:~] esxcli software sources profile list -d
.ash_history      bootpart.gz       locker/           sbin/             tmp/              vmupgrade
.mtoolsrc         dev/              mbr/              scratch/          usr/
altbootbank/      etc/              opt/              store/            var/
bin/              lib/              proc/             tardisks.noauto/  vmfs/
bootbank/         lib64/            productLocker/    tardisks/         vmimages/
[root@localhost:~] esxcli software sources profile list -d /store/
packages/  var/
[root@localhost:~] esxcli software sources profile list -d /vm
vmfs/      vmimages/  vmupgrade
[root@localhost:~] esxcli software sources profile list -d /vmfs/
devices/  volumes/
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/
5c06489e-4789b917-3280-0022196549d7/  5c0648ab-3c5aef19-2cb4-0022196549d7/  e26d8e78-af096493-61d8-d64371093be8/
5c0648a6-41ce023b-f39f-0022196549d7/  datastore1/                           f959b178-332d66e5-88e2-0a72e7fee701/
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/
5c06489e-4789b917-3280-0022196549d7/  5c0648ab-3c5aef19-2cb4-0022196549d7/  e26d8e78-af096493-61d8-d64371093be8/
5c0648a6-41ce023b-f39f-0022196549d7/  datastore1/                           f959b178-332d66e5-88e2-0a72e7fee701/
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/
.fbb.sf      .fdc.sf      .pb2.sf      .pbc.sf      .sbc.sf      .sdd.sf/     .vh.sf       datastore1/
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/datastore1/update-from-esxi6.5-6.5
_update02.zip
Name                              Vendor        Acceptance Level
--------------------------------  ------------  ----------------
ESXi-6.5.0-20180501001s-standard  VMware, Inc.  PartnerSupported
ESXi-6.5.0-20180501001s-no-tools  VMware, Inc.  PartnerSupported
ESXi-6.5.0-20180502001-no-tools   VMware, Inc.  PartnerSupported
ESXi-6.5.0-20180502001-standard   VMware, Inc.  PartnerSupported
[root@localhost:~] esxcli software sources profile update -p ESXi-6.5.0-20180502001-standard -d /vmfs/volumes/datastor
e1/datastore1/update-from-esxi6.5-6.5_update02.zip
Error: Unknown command or namespace software sources profile update

[root@localhost:~] esxcli software profile update -p ESXi-6.5.0-20180502001-standard -d /vmfs/volumes/datastore1/datas
tore1/update-from-esxi6.5-6.5_update02.zip
Update Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
   VIBs Installed: VMW_bootbank_ata-libata-92_3.00.9.2-16vmw.650.0.0.4564106, VMW_bootbank_ata-pata-amd_0.3.10-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-atiixp_0.4.6-4vmw.650.0.0.4564106, VMW_bootbank_ata-pata-cmd64x_0.2.5-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-pdc2027x_1.0-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-serverworks_0.4.3-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-sil680_0.4.8-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-via_0.3.3-2vmw.650.0.0.4564106, VMW_bootbank_block-cciss_3.6.14-10vmw.650.0.0.4564106, VMW_bootbank_bnxtnet_20.6.101.7-11vmw.650.2.50.8294253, VMW_bootbank_brcmfcoe_11.4.1078.0-8vmw.650.2.50.8294253, VMW_bootbank_char-random_1.0-3vmw.650.0.0.4564106, VMW_bootbank_ehci-ehci-hcd_1.0-4vmw.650.0.14.5146846, VMW_bootbank_elxnet_11.1.91.0-1vmw.650.0.0.4564106, VMW_bootbank_hid-hid_1.0-3vmw.650.0.0.4564106, VMW_bootbank_i40en_1.3.1-19vmw.650.2.50.8294253, VMW_bootbank_igbn_0.1.0.0-15vmw.650.1.36.7388607, VMW_bootbank_ipmi-ipmi-devintf_39.1-5vmw.650.2.50.8294253, VMW_bootbank_ipmi-ipmi-msghandler_39.1-5vmw.650.2.50.8294253, VMW_bootbank_ipmi-ipmi-si-drv_39.1-4vmw.650.0.0.4564106, VMW_bootbank_ixgben_1.4.1-12vmw.650.2.50.8294253, VMW_bootbank_lpfc_11.4.33.1-6vmw.650.2.50.8294253, VMW_bootbank_lsi-mr3_7.702.13.00-3vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt2_20.00.01.00-4vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt35_03.00.01.00-9vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt3_16.00.01.00-1vmw.650.2.50.8294253, VMW_bootbank_misc-drivers_6.5.0-2.50.8294253, VMW_bootbank_mtip32xx-native_3.9.5-1vmw.650.0.0.4564106, VMW_bootbank_ne1000_0.8.3-7vmw.650.2.50.8294253, VMW_bootbank_nenic_1.0.0.2-1vmw.650.0.0.4564106, VMW_bootbank_net-cdc-ether_1.0-3vmw.650.0.0.4564106, VMW_bootbank_net-e1000_8.0.3.1-5vmw.650.0.0.4564106, VMW_bootbank_net-e1000e_3.2.2.1-2vmw.650.0.0.4564106, VMW_bootbank_net-enic_2.1.2.38-2vmw.650.0.0.4564106, VMW_bootbank_net-fcoe_1.0.29.9.3-7vmw.650.0.0.4564106, VMW_bootbank_net-forcedeth_0.61-2vmw.650.0.0.4564106, VMW_bootbank_net-libfcoe-92_1.0.24.9.4-8vmw.650.0.0.4564106, VMW_bootbank_net-mlx4-core_1.9.7.0-1vmw.650.0.0.4564106, VMW_bootbank_net-mlx4-en_1.9.7.0-1vmw.650.0.0.4564106, VMW_bootbank_net-nx-nic_5.0.621-5vmw.650.0.0.4564106, VMW_bootbank_net-usbnet_1.0-3vmw.650.0.0.4564106, VMW_bootbank_net-vmxnet3_1.1.3.0-3vmw.650.0.0.4564106, VMW_bootbank_nhpsa_2.0.22-3vmw.650.2.50.8294253, VMW_bootbank_nmlx4-core_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx4-en_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx4-rdma_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx5-core_4.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_ntg3_4.1.3.0-1vmw.650.1.36.7388607, VMW_bootbank_nvme_1.2.1.34-1vmw.650.2.50.8294253, VMW_bootbank_nvmxnet3_2.0.0.23-1vmw.650.1.36.7388607, VMW_bootbank_ohci-usb-ohci_1.0-3vmw.650.0.0.4564106, VMW_bootbank_pvscsi_0.1-1vmw.650.1.26.5969303, VMW_bootbank_qedentv_2.0.6.4-8vmw.650.2.50.8294253, VMW_bootbank_qfle3_1.0.2.7-1vmw.650.0.0.4564106, VMW_bootbank_qflge_1.1.0.3-1vmw.650.0.0.4564106, VMW_bootbank_qlnativefc_2.1.50.0-1vmw.650.1.26.5969303, VMW_bootbank_sata-ahci_3.0-26vmw.650.1.26.5969303, VMW_bootbank_sata-ata-piix_2.12-10vmw.650.0.0.4564106, VMW_bootbank_sata-sata-nv_3.5-4vmw.650.0.0.4564106, VMW_bootbank_sata-sata-promise_2.12-3vmw.650.0.0.4564106, VMW_bootbank_sata-sata-sil24_1.1-1vmw.650.0.0.4564106, VMW_bootbank_sata-sata-sil_2.3-4vmw.650.0.0.4564106, VMW_bootbank_sata-sata-svw_2.3-3vmw.650.0.0.4564106, VMW_bootbank_scsi-aacraid_1.1.5.1-9vmw.650.0.0.4564106, VMW_bootbank_scsi-adp94xx_1.0.8.12-6vmw.650.0.0.4564106, VMW_bootbank_scsi-aic79xx_3.1-5vmw.650.0.0.4564106, VMW_bootbank_scsi-fnic_1.5.0.45-3vmw.650.0.0.4564106, VMW_bootbank_scsi-hpsa_6.0.0.84-1vmw.650.0.0.4564106, VMW_bootbank_scsi-ips_7.12.05-4vmw.650.0.0.4564106, VMW_bootbank_scsi-iscsi-linux-92_1.0.0.2-3vmw.650.0.0.4564106, VMW_bootbank_scsi-libfc-92_1.0.40.9.3-5vmw.650.0.0.4564106, VMW_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.650.0.0.4564106, VMW_bootbank_scsi-megaraid2_2.00.4-9vmw.650.0.0.4564106, VMW_bootbank_scsi-mpt2sas_19.00.00.00-1vmw.650.0.0.4564106, VMW_bootbank_scsi-mptsas_4.23.01.00-10vmw.650.0.0.4564106, VMW_bootbank_scsi-mptspi_4.23.01.00-10vmw.650.0.0.4564106, VMW_bootbank_shim-iscsi-linux-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-iscsi-linux-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libata-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libata-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfc-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfc-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfcoe-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfcoe-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-3-0_6.5.0-0.0.4564106, VMW_bootbank_smartpqi_1.0.1.553-10vmw.650.2.50.8294253, VMW_bootbank_uhci-usb-uhci_1.0-3vmw.650.0.0.4564106, VMW_bootbank_usb-storage-usb-storage_1.0-3vmw.650.0.0.4564106, VMW_bootbank_usbcore-usb_1.0-3vmw.650.2.50.8294253, VMW_bootbank_vmkata_0.1-1vmw.650.1.36.7388607, VMW_bootbank_vmkplexer-vmkplexer_6.5.0-0.0.4564106, VMW_bootbank_vmkusb_0.1-1vmw.650.2.50.8294253, VMW_bootbank_vmw-ahci_1.1.1-1vmw.650.2.50.8294253, VMW_bootbank_xhci-xhci_1.0-3vmw.650.0.0.4564106, VMware_bootbank_cpu-microcode_6.5.0-1.41.7967591, VMware_bootbank_emulex-esx-elxnetcli_11.1.28.0-0.0.4564106, VMware_bootbank_esx-base_6.5.0-2.50.8294253, VMware_bootbank_esx-dvfilter-generic-fastpath_6.5.0-1.36.7388607, VMware_bootbank_esx-tboot_6.5.0-2.50.8294253, VMware_bootbank_esx-ui_1.27.1-7909286, VMware_bootbank_esx-xserver_6.5.0-2.50.8294253, VMware_bootbank_lsu-hp-hpsa-plugin_2.0.0-6vmw.650.2.50.8294253, VMware_bootbank_lsu-lsi-lsi-mr3-plugin_1.0.0-10vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-lsi-msgpt3-plugin_1.0.0-7vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-megaraid-sas-plugin_1.0.0-8vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-mpt2sas-plugin_2.0.0-6vmw.650.1.26.5969303, VMware_bootbank_native-misc-drivers_6.5.0-0.0.4564106, VMware_bootbank_rste_2.0.2.0088-4vmw.650.0.0.4564106, VMware_bootbank_vmware-esx-esxcli-nvme-plugin_1.2.0.32-2.50.8294253, VMware_bootbank_vsan_6.5.0-2.50.8064065, VMware_bootbank_vsanhealth_6.5.0-2.50.8143339, VMware_locker_tools-light_6.5.0-1.47.8285314
   VIBs Removed: Avago_bootbank_lsi-mr3_6.606.12.00-1OEM.600.0.0.2159203, EMU_bootbank_elxnet_10.6.126.0-1OEM.600.0.0.2159203, EMU_bootbank_lpfc_10.6.126.0-1OEM.600.0.0.2159203, MEL_bootbank_nmlx4-core_3.1.0.0-1OEM.600.0.0.2348722, MEL_bootbank_nmlx4-en_3.1.0.0-1OEM.600.0.0.2348722, QLogic_bootbank_qlnativefc_2.1.24.0-1OEM.600.0.0.2768847, VMWARE_bootbank_mtip32xx-native_3.8.5-1vmw.600.0.0.2494585, VMware_bootbank_ata-pata-amd_0.3.10-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-atiixp_0.4.6-4vmw.600.0.0.2494585, VMware_bootbank_ata-pata-cmd64x_0.2.5-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-pdc2027x_1.0-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-serverworks_0.4.3-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-sil680_0.4.8-3vmw.600.0.0.2494585, VMware_bootbank_ata-pata-via_0.3.3-2vmw.600.0.0.2494585, VMware_bootbank_block-cciss_3.6.14-10vmw.600.0.0.2494585, VMware_bootbank_cpu-microcode_6.0.0-0.0.2494585, VMware_bootbank_ehci-ehci-hcd_1.0-3vmw.600.0.0.2494585, VMware_bootbank_emulex-esx-elxnetcli_10.2.309.6v-0.0.2494585, VMware_bootbank_esx-base_6.0.0-1.20.3073146, VMware_bootbank_esx-dvfilter-generic-fastpath_6.0.0-0.0.2494585, VMware_bootbank_esx-tboot_6.0.0-0.0.2494585, VMware_bootbank_esx-xserver_6.0.0-0.0.2494585, VMware_bootbank_ipmi-ipmi-devintf_39.1-4vmw.600.0.0.2494585, VMware_bootbank_ipmi-ipmi-msghandler_39.1-4vmw.600.0.0.2494585, VMware_bootbank_ipmi-ipmi-si-drv_39.1-4vmw.600.0.0.2494585, VMware_bootbank_lsi-msgpt3_06.255.12.00-8vmw.600.1.17.3029758, VMware_bootbank_lsu-hp-hpsa-plugin_1.0.0-1vmw.600.0.0.2494585, VMware_bootbank_lsu-lsi-lsi-mr3-plugin_1.0.0-2vmw.600.0.11.2809209, VMware_bootbank_lsu-lsi-lsi-msgpt3-plugin_1.0.0-1vmw.600.0.0.2494585, VMware_bootbank_lsu-lsi-megaraid-sas-plugin_1.0.0-2vmw.600.0.11.2809209, VMware_bootbank_lsu-lsi-mpt2sas-plugin_1.0.0-4vmw.600.1.17.3029758, VMware_bootbank_misc-drivers_6.0.0-1.17.3029758, VMware_bootbank_net-e1000_8.0.3.1-5vmw.600.0.0.2494585, VMware_bootbank_net-e1000e_2.5.4-6vmw.600.0.0.2494585, VMware_bootbank_net-enic_2.1.2.38-2vmw.600.0.0.2494585, VMware_bootbank_net-forcedeth_0.61-2vmw.600.0.0.2494585, VMware_bootbank_net-mlx4-core_1.9.7.0-1vmw.600.0.0.2494585, VMware_bootbank_net-mlx4-en_1.9.7.0-1vmw.600.0.0.2494585, VMware_bootbank_net-nx-nic_5.0.621-5vmw.600.0.0.2494585, VMware_bootbank_net-vmxnet3_1.1.3.0-3vmw.600.0.0.2494585, VMware_bootbank_nmlx4-rdma_3.0.0.0-1vmw.600.0.0.2494585, VMware_bootbank_nvme_1.0e.0.35-1vmw.600.1.17.3029758, VMware_bootbank_ohci-usb-ohci_1.0-3vmw.600.0.0.2494585, VMware_bootbank_rste_2.0.2.0088-4vmw.600.0.0.2494585, VMware_bootbank_sata-ahci_3.0-22vmw.600.1.17.3029758, VMware_bootbank_sata-ata-piix_2.12-10vmw.600.0.0.2494585, VMware_bootbank_sata-sata-nv_3.5-4vmw.600.0.0.2494585, VMware_bootbank_sata-sata-promise_2.12-3vmw.600.0.0.2494585, VMware_bootbank_sata-sata-sil24_1.1-1vmw.600.0.0.2494585, VMware_bootbank_sata-sata-sil_2.3-4vmw.600.0.0.2494585, VMware_bootbank_sata-sata-svw_2.3-3vmw.600.0.0.2494585, VMware_bootbank_scsi-aacraid_1.1.5.1-9vmw.600.0.0.2494585, VMware_bootbank_scsi-adp94xx_1.0.8.12-6vmw.600.0.0.2494585, VMware_bootbank_scsi-aic79xx_3.1-5vmw.600.0.0.2494585, VMware_bootbank_scsi-fnic_1.5.0.45-3vmw.600.0.0.2494585, VMware_bootbank_scsi-hpsa_6.0.0.44-4vmw.600.0.0.2494585, VMware_bootbank_scsi-ips_7.12.05-4vmw.600.0.0.2494585, VMware_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.600.0.0.2494585, VMware_bootbank_scsi-megaraid2_2.00.4-9vmw.600.0.0.2494585, VMware_bootbank_scsi-mpt2sas_19.00.00.00-1vmw.600.0.0.2494585, VMware_bootbank_scsi-mptsas_4.23.01.00-9vmw.600.0.0.2494585, VMware_bootbank_scsi-mptspi_4.23.01.00-9vmw.600.0.0.2494585, VMware_bootbank_uhci-usb-uhci_1.0-3vmw.600.0.0.2494585, VMware_bootbank_vsanhealth_6.0.0-3000000.2.0.1.17.2972216, VMware_bootbank_xhci-xhci_1.0-2vmw.600.1.17.3029758, VMware_locker_tools-light_6.0.0-1.17.3029758
   VIBs Skipped: VMW_bootbank_ima-qla4xxx_2.02.18-1vmw.650.0.0.4564106, VMW_bootbank_misc-cnic-register_1.78.75.v60.7-1vmw.650.0.0.4564106, VMW_bootbank_net-bnx2_2.2.4f.v60.10-2vmw.650.0.0.4564106, VMW_bootbank_net-bnx2x_1.78.80.v60.12-1vmw.650.0.0.4564106, VMW_bootbank_net-cnic_1.78.76.v60.13-2vmw.650.0.0.4564106, VMW_bootbank_net-igb_5.0.5.1.1-5vmw.650.0.0.4564106, VMW_bootbank_net-ixgbe_3.7.13.7.14iov-20vmw.650.0.0.4564106, VMW_bootbank_net-tg3_3.131d.v60.4-2vmw.650.0.0.4564106, VMW_bootbank_scsi-bnx2fc_1.78.78.v60.8-1vmw.650.0.0.4564106, VMW_bootbank_scsi-bnx2i_2.78.76.v60.8-1vmw.650.0.0.4564106, VMW_bootbank_scsi-megaraid-sas_6.603.55.00-2vmw.650.0.0.4564106, VMW_bootbank_scsi-qla4xxx_5.01.03.2-7vmw.650.0.0.4564106
[root@localhost:~]
```

## 6.5 -> 6.7 update log

```
[root@localhost:~] esxcli software sources profile list -d /vmfs/
devices/  volumes/
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/datastore1/update-from-esxi6.
update-from-esxi6.5-6.5_update02.zip  update-from-esxi6.7-6.7_update01.zip
[root@localhost:~] esxcli software sources profile list -d /vmfs/volumes/datastore1/datastore1/update-from-esxi6.7-6.7
_update01.zip
Name                              Vendor        Acceptance Level  Creation Time        Modification Time
--------------------------------  ------------  ----------------  -------------------  -------------------
ESXi-6.7.0-20181001001s-no-tools  VMware, Inc.  PartnerSupported  2018-10-08T10:26:44  2018-10-08T10:26:44
ESXi-6.7.0-20181002001-no-tools   VMware, Inc.  PartnerSupported  2018-10-08T10:26:44  2018-10-08T10:26:44
ESXi-6.7.0-20181002001-standard   VMware, Inc.  PartnerSupported  2018-10-08T10:26:44  2018-10-08T10:26:44
ESXi-6.7.0-20181001001s-standard  VMware, Inc.  PartnerSupported  2018-10-08T10:26:44  2018-10-08T10:26:44
[root@localhost:~] esxcli software profile update -p ESXi-6.7.0-20181002001-standard -d /vmfs/volumes/datastore1/datas
tore1/update-from-esxi6.7-6.7_update01.zip
Update Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
   VIBs Installed: VMW_bootbank_ata-libata-92_3.00.9.2-16vmw.670.0.0.8169922, VMW_bootbank_ata-pata-amd_0.3.10-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-atiixp_0.4.6-4vmw.670.0.0.8169922, VMW_bootbank_ata-pata-cmd64x_0.2.5-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-pdc2027x_1.0-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-serverworks_0.4.3-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-sil680_0.4.8-3vmw.670.0.0.8169922, VMW_bootbank_ata-pata-via_0.3.3-2vmw.670.0.0.8169922, VMW_bootbank_block-cciss_3.6.14-10vmw.670.0.0.8169922, VMW_bootbank_bnxtnet_20.6.101.7-11vmw.670.0.0.8169922, VMW_bootbank_bnxtroce_20.6.101.0-20vmw.670.1.28.10302608, VMW_bootbank_brcmfcoe_11.4.1078.5-11vmw.670.1.28.10302608, VMW_bootbank_char-random_1.0-3vmw.670.0.0.8169922, VMW_bootbank_ehci-ehci-hcd_1.0-4vmw.670.0.0.8169922, VMW_bootbank_elxiscsi_11.4.1174.0-2vmw.670.0.0.8169922, VMW_bootbank_elxnet_11.4.1095.0-5vmw.670.1.28.10302608, VMW_bootbank_hid-hid_1.0-3vmw.670.0.0.8169922, VMW_bootbank_i40en_1.3.1-22vmw.670.1.28.10302608, VMW_bootbank_iavmd_1.2.0.1011-2vmw.670.0.0.8169922, VMW_bootbank_igbn_0.1.0.0-15vmw.670.0.0.8169922, VMW_bootbank_ipmi-ipmi-devintf_39.1-5vmw.670.1.28.10302608, VMW_bootbank_ipmi-ipmi-msghandler_39.1-5vmw.670.1.28.10302608, VMW_bootbank_ipmi-ipmi-si-drv_39.1-5vmw.670.1.28.10302608, VMW_bootbank_iser_1.0.0.0-1vmw.670.1.28.10302608, VMW_bootbank_ixgben_1.4.1-16vmw.670.1.28.10302608, VMW_bootbank_lpfc_11.4.33.3-11vmw.670.1.28.10302608, VMW_bootbank_lpnic_11.4.59.0-1vmw.670.0.0.8169922, VMW_bootbank_lsi-mr3_7.702.13.00-5vmw.670.1.28.10302608, VMW_bootbank_lsi-msgpt2_20.00.04.00-5vmw.670.1.28.10302608, VMW_bootbank_lsi-msgpt35_03.00.01.00-12vmw.670.1.28.10302608, VMW_bootbank_lsi-msgpt3_16.00.01.00-3vmw.670.1.28.10302608, VMW_bootbank_misc-drivers_6.7.0-0.0.8169922, VMW_bootbank_mtip32xx-native_3.9.8-1vmw.670.1.28.10302608, VMW_bootbank_ne1000_0.8.4-1vmw.670.1.28.10302608, VMW_bootbank_nenic_1.0.21.0-1vmw.670.1.28.10302608, VMW_bootbank_net-cdc-ether_1.0-3vmw.670.0.0.8169922, VMW_bootbank_net-e1000_8.0.3.1-5vmw.670.0.0.8169922, VMW_bootbank_net-e1000e_3.2.2.1-2vmw.670.0.0.8169922, VMW_bootbank_net-enic_2.1.2.38-2vmw.670.0.0.8169922, VMW_bootbank_net-fcoe_1.0.29.9.3-7vmw.670.0.0.8169922, VMW_bootbank_net-forcedeth_0.61-2vmw.670.0.0.8169922, VMW_bootbank_net-libfcoe-92_1.0.24.9.4-8vmw.670.0.0.8169922, VMW_bootbank_net-mlx4-core_1.9.7.0-1vmw.670.0.0.8169922, VMW_bootbank_net-mlx4-en_1.9.7.0-1vmw.670.0.0.8169922, VMW_bootbank_net-nx-nic_5.0.621-5vmw.670.0.0.8169922, VMW_bootbank_net-usbnet_1.0-3vmw.670.0.0.8169922, VMW_bootbank_net-vmxnet3_1.1.3.0-3vmw.670.0.0.8169922, VMW_bootbank_nfnic_4.0.0.14-0vmw.670.1.28.10302608, VMW_bootbank_nhpsa_2.0.22-3vmw.670.1.28.10302608, VMW_bootbank_nmlx4-core_3.17.9.12-1vmw.670.0.0.8169922, VMW_bootbank_nmlx4-en_3.17.9.12-1vmw.670.0.0.8169922, VMW_bootbank_nmlx4-rdma_3.17.9.12-1vmw.670.0.0.8169922, VMW_bootbank_nmlx5-core_4.17.9.12-1vmw.670.0.0.8169922, VMW_bootbank_nmlx5-rdma_4.17.9.12-1vmw.670.0.0.8169922, VMW_bootbank_ntg3_4.1.3.2-1vmw.670.1.28.10302608, VMW_bootbank_nvme_1.2.2.17-1vmw.670.1.28.10302608, VMW_bootbank_nvmxnet3-ens_2.0.0.21-1vmw.670.0.0.8169922, VMW_bootbank_nvmxnet3_2.0.0.29-1vmw.670.1.28.10302608, VMW_bootbank_ohci-usb-ohci_1.0-3vmw.670.0.0.8169922, VMW_bootbank_pvscsi_0.1-2vmw.670.0.0.8169922, VMW_bootbank_qcnic_1.0.2.0.4-1vmw.670.0.0.8169922, VMW_bootbank_qedentv_2.0.6.4-10vmw.670.1.28.10302608, VMW_bootbank_qfle3_1.0.50.11-9vmw.670.0.0.8169922, VMW_bootbank_qfle3f_1.0.25.0.2-14vmw.670.0.0.8169922, VMW_bootbank_qfle3i_1.0.2.3.9-3vmw.670.0.0.8169922, VMW_bootbank_qflge_1.1.0.11-1vmw.670.0.0.8169922, VMW_bootbank_sata-ahci_3.0-26vmw.670.0.0.8169922, VMW_bootbank_sata-ata-piix_2.12-10vmw.670.0.0.8169922, VMW_bootbank_sata-sata-nv_3.5-4vmw.670.0.0.8169922, VMW_bootbank_sata-sata-promise_2.12-3vmw.670.0.0.8169922, VMW_bootbank_sata-sata-sil24_1.1-1vmw.670.0.0.8169922, VMW_bootbank_sata-sata-sil_2.3-4vmw.670.0.0.8169922, VMW_bootbank_sata-sata-svw_2.3-3vmw.670.0.0.8169922, VMW_bootbank_scsi-aacraid_1.1.5.1-9vmw.670.0.0.8169922, VMW_bootbank_scsi-adp94xx_1.0.8.12-6vmw.670.0.0.8169922, VMW_bootbank_scsi-aic79xx_3.1-6vmw.670.0.0.8169922, VMW_bootbank_scsi-fnic_1.5.0.45-3vmw.670.0.0.8169922, VMW_bootbank_scsi-hpsa_6.0.0.84-3vmw.670.0.0.8169922, VMW_bootbank_scsi-ips_7.12.05-4vmw.670.0.0.8169922, VMW_bootbank_scsi-iscsi-linux-92_1.0.0.2-3vmw.670.0.0.8169922, VMW_bootbank_scsi-libfc-92_1.0.40.9.3-5vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid2_2.00.4-9vmw.670.0.0.8169922, VMW_bootbank_scsi-mpt2sas_19.00.00.00-2vmw.670.0.0.8169922, VMW_bootbank_scsi-mptsas_4.23.01.00-10vmw.670.0.0.8169922, VMW_bootbank_scsi-mptspi_4.23.01.00-10vmw.670.0.0.8169922, VMW_bootbank_shim-iscsi-linux-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-iscsi-linux-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libata-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libata-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfc-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfc-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfcoe-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-libfcoe-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-1-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-2-0_6.7.0-0.0.8169922, VMW_bootbank_shim-vmklinux-9-2-3-0_6.7.0-0.0.8169922, VMW_bootbank_smartpqi_1.0.1.553-12vmw.670.1.28.10302608, VMW_bootbank_uhci-usb-uhci_1.0-3vmw.670.0.0.8169922, VMW_bootbank_usb-storage-usb-storage_1.0-3vmw.670.0.0.8169922, VMW_bootbank_usbcore-usb_1.0-3vmw.670.0.0.8169922, VMW_bootbank_vmkata_0.1-1vmw.670.0.0.8169922, VMW_bootbank_vmkfcoe_1.0.0.1-1vmw.670.1.28.10302608, VMW_bootbank_vmkplexer-vmkplexer_6.7.0-0.0.8169922, VMW_bootbank_vmkusb_0.1-1vmw.670.1.28.10302608, VMW_bootbank_vmw-ahci_1.2.3-1vmw.670.1.28.10302608, VMW_bootbank_xhci-xhci_1.0-3vmw.670.0.0.8169922, VMware_bootbank_cpu-microcode_6.7.0-1.28.10302608, VMware_bootbank_elx-esx-libelxima.so_11.4.1184.0-0.0.8169922, VMware_bootbank_esx-base_6.7.0-1.28.10302608, VMware_bootbank_esx-dvfilter-generic-fastpath_6.7.0-0.0.8169922, VMware_bootbank_esx-ui_1.30.0-9946814, VMware_bootbank_esx-update_6.7.0-1.28.10302608, VMware_bootbank_esx-xserver_6.7.0-0.0.8169922, VMware_bootbank_lsu-hp-hpsa-plugin_2.0.0-16vmw.670.1.28.10302608, VMware_bootbank_lsu-intel-vmd-plugin_1.0.0-2vmw.670.1.28.10302608, VMware_bootbank_lsu-lsi-lsi-mr3-plugin_1.0.0-13vmw.670.1.28.10302608, VMware_bootbank_lsu-lsi-lsi-msgpt3-plugin_1.0.0-8vmw.670.0.0.8169922, VMware_bootbank_lsu-lsi-megaraid-sas-plugin_1.0.0-9vmw.670.0.0.8169922, VMware_bootbank_lsu-lsi-mpt2sas-plugin_2.0.0-7vmw.670.0.0.8169922, VMware_bootbank_lsu-smartpqi-plugin_1.0.0-3vmw.670.1.28.10302608, VMware_bootbank_native-misc-drivers_6.7.0-0.0.8169922, VMware_bootbank_qlnativefc_3.0.1.0-5vmw.670.0.0.8169922, VMware_bootbank_rste_2.0.2.0088-7vmw.670.0.0.8169922, VMware_bootbank_vmware-esx-esxcli-nvme-plugin_1.2.0.34-1.28.10302608, VMware_bootbank_vsan_6.7.0-1.28.10290435, VMware_bootbank_vsanhealth_6.7.0-1.28.10290721, VMware_locker_tools-light_10.3.2.9925305-10176879
   VIBs Removed: EMU_bootbank_ima-be2iscsi_10.6.150.3-1OEM.600.0.0.2159203, EMU_bootbank_scsi-be2iscsi_10.6.150.3-1OEM.600.0.0.2159203, VMW_bootbank_ata-libata-92_3.00.9.2-16vmw.650.0.0.4564106, VMW_bootbank_ata-pata-amd_0.3.10-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-atiixp_0.4.6-4vmw.650.0.0.4564106, VMW_bootbank_ata-pata-cmd64x_0.2.5-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-pdc2027x_1.0-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-serverworks_0.4.3-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-sil680_0.4.8-3vmw.650.0.0.4564106, VMW_bootbank_ata-pata-via_0.3.3-2vmw.650.0.0.4564106, VMW_bootbank_block-cciss_3.6.14-10vmw.650.0.0.4564106, VMW_bootbank_bnxtnet_20.6.101.7-11vmw.650.2.50.8294253, VMW_bootbank_brcmfcoe_11.4.1078.0-8vmw.650.2.50.8294253, VMW_bootbank_char-random_1.0-3vmw.650.0.0.4564106, VMW_bootbank_ehci-ehci-hcd_1.0-4vmw.650.0.14.5146846, VMW_bootbank_elxnet_11.1.91.0-1vmw.650.0.0.4564106, VMW_bootbank_hid-hid_1.0-3vmw.650.0.0.4564106, VMW_bootbank_i40en_1.3.1-19vmw.650.2.50.8294253, VMW_bootbank_igbn_0.1.0.0-15vmw.650.1.36.7388607, VMW_bootbank_ipmi-ipmi-devintf_39.1-5vmw.650.2.50.8294253, VMW_bootbank_ipmi-ipmi-msghandler_39.1-5vmw.650.2.50.8294253, VMW_bootbank_ipmi-ipmi-si-drv_39.1-4vmw.650.0.0.4564106, VMW_bootbank_ixgben_1.4.1-12vmw.650.2.50.8294253, VMW_bootbank_lpfc_11.4.33.1-6vmw.650.2.50.8294253, VMW_bootbank_lsi-mr3_7.702.13.00-3vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt2_20.00.01.00-4vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt35_03.00.01.00-9vmw.650.2.50.8294253, VMW_bootbank_lsi-msgpt3_16.00.01.00-1vmw.650.2.50.8294253, VMW_bootbank_misc-drivers_6.5.0-2.50.8294253, VMW_bootbank_mtip32xx-native_3.9.5-1vmw.650.0.0.4564106, VMW_bootbank_ne1000_0.8.3-7vmw.650.2.50.8294253, VMW_bootbank_nenic_1.0.0.2-1vmw.650.0.0.4564106, VMW_bootbank_net-cdc-ether_1.0-3vmw.650.0.0.4564106, VMW_bootbank_net-e1000_8.0.3.1-5vmw.650.0.0.4564106, VMW_bootbank_net-e1000e_3.2.2.1-2vmw.650.0.0.4564106, VMW_bootbank_net-enic_2.1.2.38-2vmw.650.0.0.4564106, VMW_bootbank_net-fcoe_1.0.29.9.3-7vmw.650.0.0.4564106, VMW_bootbank_net-forcedeth_0.61-2vmw.650.0.0.4564106, VMW_bootbank_net-libfcoe-92_1.0.24.9.4-8vmw.650.0.0.4564106, VMW_bootbank_net-mlx4-core_1.9.7.0-1vmw.650.0.0.4564106, VMW_bootbank_net-mlx4-en_1.9.7.0-1vmw.650.0.0.4564106, VMW_bootbank_net-nx-nic_5.0.621-5vmw.650.0.0.4564106, VMW_bootbank_net-usbnet_1.0-3vmw.650.0.0.4564106, VMW_bootbank_net-vmxnet3_1.1.3.0-3vmw.650.0.0.4564106, VMW_bootbank_nhpsa_2.0.22-3vmw.650.2.50.8294253, VMW_bootbank_nmlx4-core_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx4-en_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx4-rdma_3.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_nmlx5-core_4.16.0.0-1vmw.650.0.0.4564106, VMW_bootbank_ntg3_4.1.3.0-1vmw.650.1.36.7388607, VMW_bootbank_nvme_1.2.1.34-1vmw.650.2.50.8294253, VMW_bootbank_nvmxnet3_2.0.0.23-1vmw.650.1.36.7388607, VMW_bootbank_ohci-usb-ohci_1.0-3vmw.650.0.0.4564106, VMW_bootbank_pvscsi_0.1-1vmw.650.1.26.5969303, VMW_bootbank_qedentv_2.0.6.4-8vmw.650.2.50.8294253, VMW_bootbank_qfle3_1.0.2.7-1vmw.650.0.0.4564106, VMW_bootbank_qflge_1.1.0.3-1vmw.650.0.0.4564106, VMW_bootbank_qlnativefc_2.1.50.0-1vmw.650.1.26.5969303, VMW_bootbank_sata-ahci_3.0-26vmw.650.1.26.5969303, VMW_bootbank_sata-ata-piix_2.12-10vmw.650.0.0.4564106, VMW_bootbank_sata-sata-nv_3.5-4vmw.650.0.0.4564106, VMW_bootbank_sata-sata-promise_2.12-3vmw.650.0.0.4564106, VMW_bootbank_sata-sata-sil24_1.1-1vmw.650.0.0.4564106, VMW_bootbank_sata-sata-sil_2.3-4vmw.650.0.0.4564106, VMW_bootbank_sata-sata-svw_2.3-3vmw.650.0.0.4564106, VMW_bootbank_scsi-aacraid_1.1.5.1-9vmw.650.0.0.4564106, VMW_bootbank_scsi-adp94xx_1.0.8.12-6vmw.650.0.0.4564106, VMW_bootbank_scsi-aic79xx_3.1-5vmw.650.0.0.4564106, VMW_bootbank_scsi-fnic_1.5.0.45-3vmw.650.0.0.4564106, VMW_bootbank_scsi-hpsa_6.0.0.84-1vmw.650.0.0.4564106, VMW_bootbank_scsi-ips_7.12.05-4vmw.650.0.0.4564106, VMW_bootbank_scsi-iscsi-linux-92_1.0.0.2-3vmw.650.0.0.4564106, VMW_bootbank_scsi-libfc-92_1.0.40.9.3-5vmw.650.0.0.4564106, VMW_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.650.0.0.4564106, VMW_bootbank_scsi-megaraid2_2.00.4-9vmw.650.0.0.4564106, VMW_bootbank_scsi-mpt2sas_19.00.00.00-1vmw.650.0.0.4564106, VMW_bootbank_scsi-mptsas_4.23.01.00-10vmw.650.0.0.4564106, VMW_bootbank_scsi-mptspi_4.23.01.00-10vmw.650.0.0.4564106, VMW_bootbank_shim-iscsi-linux-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-iscsi-linux-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libata-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libata-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfc-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfc-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfcoe-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-libfcoe-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-1-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-2-0_6.5.0-0.0.4564106, VMW_bootbank_shim-vmklinux-9-2-3-0_6.5.0-0.0.4564106, VMW_bootbank_smartpqi_1.0.1.553-10vmw.650.2.50.8294253, VMW_bootbank_uhci-usb-uhci_1.0-3vmw.650.0.0.4564106, VMW_bootbank_usb-storage-usb-storage_1.0-3vmw.650.0.0.4564106, VMW_bootbank_usbcore-usb_1.0-3vmw.650.2.50.8294253, VMW_bootbank_vmkata_0.1-1vmw.650.1.36.7388607, VMW_bootbank_vmkplexer-vmkplexer_6.5.0-0.0.4564106, VMW_bootbank_vmkusb_0.1-1vmw.650.2.50.8294253, VMW_bootbank_vmw-ahci_1.1.1-1vmw.650.2.50.8294253, VMW_bootbank_xhci-xhci_1.0-3vmw.650.0.0.4564106, VMware_bootbank_cpu-microcode_6.5.0-1.41.7967591, VMware_bootbank_emulex-esx-elxnetcli_11.1.28.0-0.0.4564106, VMware_bootbank_esx-base_6.5.0-2.50.8294253, VMware_bootbank_esx-dvfilter-generic-fastpath_6.5.0-1.36.7388607, VMware_bootbank_esx-tboot_6.5.0-2.50.8294253, VMware_bootbank_esx-ui_1.27.1-7909286, VMware_bootbank_esx-xserver_6.5.0-2.50.8294253, VMware_bootbank_lsu-hp-hpsa-plugin_2.0.0-6vmw.650.2.50.8294253, VMware_bootbank_lsu-lsi-lsi-mr3-plugin_1.0.0-10vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-lsi-msgpt3-plugin_1.0.0-7vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-megaraid-sas-plugin_1.0.0-8vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-mpt2sas-plugin_2.0.0-6vmw.650.1.26.5969303, VMware_bootbank_lsu-lsi-mptsas-plugin_1.0.0-1vmw.600.0.0.2494585, VMware_bootbank_native-misc-drivers_6.5.0-0.0.4564106, VMware_bootbank_rste_2.0.2.0088-4vmw.650.0.0.4564106, VMware_bootbank_vmware-esx-esxcli-nvme-plugin_1.2.0.32-2.50.8294253, VMware_bootbank_vsan_6.5.0-2.50.8064065, VMware_bootbank_vsanhealth_6.5.0-2.50.8143339, VMware_locker_tools-light_6.5.0-1.47.8285314
   VIBs Skipped: VMW_bootbank_ima-qla4xxx_2.02.18-1vmw.670.0.0.8169922, VMW_bootbank_misc-cnic-register_1.78.75.v60.7-1vmw.670.0.0.8169922, VMW_bootbank_net-bnx2_2.2.4f.v60.10-2vmw.670.0.0.8169922, VMW_bootbank_net-bnx2x_1.78.80.v60.12-2vmw.670.0.0.8169922, VMW_bootbank_net-cnic_1.78.76.v60.13-2vmw.670.0.0.8169922, VMW_bootbank_net-igb_5.0.5.1.1-5vmw.670.0.0.8169922, VMW_bootbank_net-ixgbe_3.7.13.7.14iov-20vmw.670.0.0.8169922, VMW_bootbank_net-tg3_3.131d.v60.4-2vmw.670.0.0.8169922, VMW_bootbank_scsi-bnx2fc_1.78.78.v60.8-1vmw.670.0.0.8169922, VMW_bootbank_scsi-bnx2i_2.78.76.v60.8-1vmw.670.0.0.8169922, VMW_bootbank_scsi-megaraid-sas_6.603.55.00-2vmw.670.0.0.8169922, VMW_bootbank_scsi-qla4xxx_5.01.03.2-7vmw.670.0.0.8169922
[root@localhost:~]
```

## Synology NAS(DS918+)とESXiをiSCSIする
わりと簡単にできたのでメモっておく．

### やりたいこと
- SynologyのNAS(DS918+)とESXi 6.5の間をiSCSIでつなぐ．
- Synology NASがtarget, ESXiがinitiator
- CHAP認証してみる．

### やりかた
- ここ見るとさくっとできる．
  - [DiskStation Manager - Knowledge Base | Synology Inc.](https://www.synology.com/ja-jp/knowledgebase/DSM/tutorial/Virtualization/How_to_set_up_Synology_NAS_as_VMware_server_datastore)
- じゃあなんでお前この記事書いてんのって話だが，自分の記録のためである．

### やる
まずはSynology NAS側でiSCSI targetを設定する．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_01.png
```
iSCSI managerからtargetを新規作成する．CHAPしたいときはCHAP有効化して認証情報をいれておく．別に後から有効化することもできる(一時的なstorage断は伴うが)．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_02.png
```
LUNが未作成の場合/割り当てるLUNがない場合はここでLUNを作成する．すでに作成済みの場合はそれを割り当てる事もできる．適当にsizeやらを設定して作成する．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_03.png
```
LUNを作成したいボリュームや容量を決める．
スペース割り当てと書いてある部分はthick/thin provisioningが選べるはず．
thick provisioningはLUN作成時に割り当て容量のすべてを実際のディスクに割り当てをするが，thinの場合は必要になったらその都度割り当てするようなイメージ．
thinの場合は利用した分だけ割り当てられるのでストレージの利用効率がいいが，その特性上必要になったら都度割り当て, zeronize等の処理が入るので若干のパフォーマンス低下が存在することがあるというのが懸念点かと思う．
それに対しthin provisioningは初めに割り当て容量の全てをzeronizeする．この処理が入るのは割り当て時のみであるため利用中の割り当て/zeronize等によるパフォーマンス低下は気にしなくていいと考えられる．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_04.png
```
見直して良ければ適用してLUN, targetの作成を終える．
これでSynology NAS側のsettingは終了．IQNとSynology NASのIPがのちに必要になる．
ここからはinitiatorとなるESXi側で設定を行なっていく．
`ストレージ > アダプタ`タブを選択し，今回はソフトウェアiSCSIを利用するのでそいつを選択．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_05.png
```
iSCSIを有効にし，CHAP認証を利用する場合はCHAPを有効化して認証情報を入れる．
ポートバインディングではiSCSIを利用するVMkernel NICを選ぶ．これはmanagement VMkernel NICとは別にしておくのがよさそう．iSCSI用にVMkernel NICをあらかじめ割り当てておくとよい．
固定ターゲットの部分にSynology NASのIQNとIP Addressを入れる．動的ターゲットとしてIP Addressを入れるだけでも接続と思われるが，より限定的/明示的に設定したいので今回は固定ターゲットとして設定する．
入力を終えたら設定を保存して適用する．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_06.png
```
設定が正常に行われるとデバイスにiSCSI接続されたNASが見える．
// degradeしているのが見えているが，これはiSCSIのuplinkが冗長でないためのよう．複数のuplinkをvswitchにいれてやれば解決すると思う．
もちろんSynology NAS側でも接続されていることが確認できる(iSCSI managerで確認可能)．
ここまでくればあとはこのiSCSI targetに対してVMFSを設定して利用するだけだ．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_07.png
```
`ストレージ > データストア > 新しいデータストア` からデータストア設定wisardに入る．
新規にVMFSを作成する．あとはぽちぽち進んで，できあがる．
```eval_rst
.. image:: ../resources/images/esxi_synology_iscsi_08.png
```
データストア もちゃんと見えている．めでたし

### 気になること．
- どのくらい切断するとVMに顕著に影響するのか．
  - もちろん切断した瞬間からファイルシステムへの書き込みはできなくなるわけだが，たとえばdbとかどうなるのかわかってない．
  - 要するにdisk busyになっちゃうからmem上でしか遊べなくなるんだろう．
- [iSCSIが切断された時どうなるか検証（ESXi） | fefcc.net](https://fefcc.net/archives/475)

### References
- [DiskStation Manager - Knowledge Base | Synology Inc.](https://www.synology.com/ja-jp/knowledgebase/DSM/tutorial/Virtualization/How_to_set_up_Synology_NAS_as_VMware_server_datastore)
- [VMware Knowledge Base](https://kb.vmware.com/s/article/2045040)
