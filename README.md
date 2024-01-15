# VM

## Install QEMU and KVM on Ubuntu
- KVM (Kernel-based Virtual Machine) already part of the kernel.
- QEMU (Quick Emulator) is a software module that emulates various components of computer hardware. It supports full virtualizations and works alongside KVM to provide a holistic virtualization experience.

### Check Virtualization Enabled in Ubuntu
- vmx : Intel VT-x
- svm : AMD-V

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```
Or type in the following command:
```bash
grep -E --color '(vmx|svm)' /proc/cpuinfo
```
```bash
sudo apt install cpu-checker -y
```
```bash
kvm-ok
```
### Packages names
- qemu-kvm : This is an open-source emulator that emulates the hardware resources of a computer.
- virt-manager : A Qt-based GUI interface for creating and managing virtual machines using the libvirt daemon.
- virtinst : A collection of command-line utilities for creating and making changes to virtual machines.
- libvirt-clients : APIs and client-side libraries for managing virtual machines from the command line.
- bridge-utils : A set of command-line tools for managing bridge devices.
- libvirt-daemon-system : Provides configuration files needed to run the virtualization service.
```bash
sudo apt update
sudo apt install <package names>
```
Start and enable the libvirtd virtualization daemon
```bash
sudo systemctl enable --now libvirtd
sudo systemctl start libvirtd
sudo systemctl status libvirtd
```

### Add user to the kvm and libvirt groups
```bash
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
```


# Run VM

### Start with the GUI
```bash
sudo virt-manager
```
### Start with the command line:
Virtuelle Festplatte einrichten:
```bash
qemu-img create testvm.img 2G 
```
Installation von CD-Image:
```bash
qemu-system-x86_64 -enable-kvm -hda testvm.img -cdrom <path>/img/ubuntu-20.10-desktop-amd64.iso -boot d -m 1024 
```

Tastenkombination von QEMU:
- Strg + Alt	: Maus aus dem QEMU-Fenster befreien
- Strg + Alt + 2	: vom Gast in den QEMU-Monitor wechseln
- Strg + Alt + 1	: vom QEMU-Monitor ins Gast-Betriebssystem wechseln
- Strg + Alt + F	: zwischen Fenster- und Vollbildmodus wechseln

## Download sources for ISOs

### Fedora
https://fedoraproject.org/de/workstation/download

### Ubuntu
https://ubuntu.com/download

