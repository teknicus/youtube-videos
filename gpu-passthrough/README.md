# Remote Gaming! (and Video Encoding using Proxmox and GPU Passthrough)

[![Remote Gaming! (and Video Encoding using Proxmox and GPU Passthrough)](http://img.youtube.com/vi/fgx3NMk6F54/0.jpg)](https://www.youtube.com/watch?v=fgx3NMk6F54 "Remote Gaming! (and Video Encoding using Proxmox and GPU Passthrough)")


Are you looking to build a remote gaming machine and passthrough your GPU to a virtual machine?  Do you want to use GPU acceleration for transcoding Plex or Adobe Media Encoder?  Do you dream of setting up Steam Link or Remote Play In Home Streaming and streaming games to any screen in your house?  If so, this complete step-by-step guide of how to passthrough your Nvidia or AMD video card through to a guest VM using Proxmox VE!  If not, well, please watch this anyway.  



edit grub

`/etc/default/grub`


Change this line:

`GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on pcie_acs_override=downstream,multifunction video=efifb:eek:ff"`

run

`update-grub`

reboot
`reboot`

Edit
`/etc/modules`

```
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

reboot
`reboot`


`qm.conf` (will be something like `100.conf`)

```
agent: 1
balloon: 4096
bios: ovmf
boot: cdn
bootdisk: virtio0
cores: 8
cpu: host,hidden=1,flags=+pcid
efidisk0: fast1:vm-100-disk-1,size=128K
hostpci0: 02:00,pcie=1,x-vga=1
hostpci1: 04:00.0,rombar=0
ide0: none,media=cdrom
machine: q35
memory: 14336
name: beam
numa: 0
ostype: win10
scsihw: virtio-scsi-pci
smbios1: uuid=d6febb0d-4242-4bdb-8aea-7c03e7b5df0e
sockets: 1
unused0: storage1:vm-100-disk-0
unused1: slow1:vm-100-disk-0
virtio0: fast1:vm-100-disk-0,size=80G
vmgenid: 524a58dd-7e3e-44f4-abf4-9de0f490d936
```

Add your PCI device


edit `/etc/modprobe.d/pve-blacklist.conf`

```
blacklist nvidiafb
blacklist nvidia
blacklist radeon
blacklist nouveau
```

## Troubleshooting

If your Windows machine fails to boot, you may want to create a new Windows VM using UEFI rather than BIOS.

https://www.reddit.com/r/homelab/comments/b5xpua/the_ultimate_beginners_guide_to_gpu_passthrough/

https://www.nvidia.com/en-us/geforce/forums/discover/207647/nvidia-amp-windows-10-how-i-solved-the-code-43-error-/

https://pve.proxmox.com/pve-docs/pve-admin-guide.html#qm_pci_passthrough

https://pve.proxmox.com/wiki/PCI(e)_Passthrough

https://forum.proxmox.com/threads/fix-for-pci-passthrough-and-issues-with-proxmox-6-gui-regex-that-needs-resolving-after-upgrade.56507/

https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Passing_through_a_device_that_does_not_support_resetting


