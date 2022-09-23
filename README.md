# Single GPU Passthrough on a Kernel Virtual Machine

This guide includes information on how to configure your virtual machine for single GPU passthrough.

## Prerequisites

This will assume you have:

- A system that is capable of PCI passthrough
- Valid IOMMU groups
- `qemu`, `libvirtd`, `virt-manager`, and `ovmf` installed

The Arch wiki includes helpful information on [requirements for PCI passthrough](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Prerequisites) and [checking your IOMMU groups](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Enabling_IOMMU).

## Set up your virtual machine

Open Virtual Machine Manager and proceed to set up your new virtual machine as normal. Before finalizing your virtual machine setup, tick the "Customize configuration before install" and press Finish. Check that "Firmware" underneath Hypervisor Details is set to UEFI. Select Boot Options and verify that your ISO image is at the top of the boot device order and then click "Begin Installation" at the top left of the window. After installing your operating system, shut down the virtual machine.

## Configure libvirt hooks

Libvirt hooks are scripts that are ran upon the VM booting and shutting down. The scripts will allow us to assign the GPU to the VM on startup by killing the display manager and restarting the display manager when the VM shuts down. The [Passthrough Post](https://passthroughpo.st/simple-per-vm-libvirt-hooks-with-the-vfio-tools-hook-helper/) has information about using hooks. You can find example start and stop scripts [here](https://github.com/joeknock90/Single-GPU-Passthrough#setting-up-libvirt-hooks). After you set up your start and stop scripts, go to your virtual machine settings and click on "Add Hardware" on the bottom left. Click on "PCI Host Device" and then add your GPU from the list.

**Note**: You may have to add your GPU's audio controller as well.

## Configure USB devices for passthrough

USB devices such as your keyboard and mouse can be configured for passthrough. You can do this by going into your virtual machine settings in Virtual Machine Manager and press on "Add Hardware" on the bottom left. Click on "USB Host Device" on the left side of the screen and add your USB device from the list on the right. The [Arch wiki](https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF#Passing_audio_from_virtual_machine_to_host_via_PulseAudio) includes information on how to pass the virtual machine's audio to the host.

## Sources

https://wiki.archlinux.org/title/PCI_passthrough_via_OVMF

https://github.com/joeknock90/Single-GPU-Passthrough

https://passthroughpo.st/simple-per-vm-libvirt-hooks-with-the-vfio-tools-hook-helper/