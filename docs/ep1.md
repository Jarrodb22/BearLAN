## Enable IOMMU
Go to bios and enable all virtualization and iommu, turn off hardware raid, turn off drive sleep. set xmp profiles, clean the host os drive.

change boot order so boot media installs from boot media in UEFI mode. Very important.

## Install proxmox os and emable iommu
after install open console:

`nano /etc/default/grub`

Look for this line:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet"
```

Then change it to look like this:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction"
```
`update-grub`

## Add vifo modules
`nano /etc/modules`

add the following:
```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```
## Confirm settings
`dmesg | grep -e DMAR -e IOMMU`

`pvesh get /nodes/{nodename}/hardware/pci --pci-class-blacklist ""`

should show IOMMU groups split.

## IOMMU Interrupt remapping 
If the Command above shows your groups split as needed then there is no reason to do the steps below.. if you are still having issues, try the blow steps, reboot and confirm again.<br>

`echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf`
`echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf`

Update and reboot

`update-initramfs -u`

`rboot now`

## Confirm settings
`dmesg | grep -e DMAR -e IOMMU`

`pvesh get /nodes/{nodename}/hardware/pci --pci-class-blacklist ""`

should show IOMMU groups split.

## Make VM for PCI passthrough
- load iso file leave os as linux (dont start on boot yet)
- change machine to q35 and bios to uefi and set LVM
- check ssd emulation and discard
- set core limit and change type to host
- set ram and uncheck ballooning
- leave the network the same and dont continue

## Passthrough IOMMU device
- go to hardware tab
- add PCIE device
- add raw device
- check add ROM do not allow full control
- save

## Install OS
- launch vm and hit esc to load bios.
- disable secure boot
- save and exit
- follow installer

## After install test 
- before going any further you can turn on start at boot and try to reboot proxmox
- if you do not have issues, the IOMMU groups held and the PCIE passthrough should be stable.
- testing this is not required just best to do it early because you will want truenas to boot wth server.

## Safe to move to headless setup
- until this point i would have been keeping the server somewhere easy to access because a failed IOMMU mount will likely mean reinstalling proxmox
- after sucessfully testing this VM i put it away in its closet to be used completely headless
