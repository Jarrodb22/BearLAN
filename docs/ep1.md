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
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt pcie_acs_override=downstream,multifunction
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

## IOMMU Interrupt remapping 
`echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf`
`echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf`

## Update and reboot
`update-initramfs -u`

`rboot now`

## Confirm settings
`dmesg | grep -e DMAR -e IOMMU`

`pvesh get /nodes/{nodename}/hardware/pci --pci-class-blacklist ""`

should show IOMMU groups split.