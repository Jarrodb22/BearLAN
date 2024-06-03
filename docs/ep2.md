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