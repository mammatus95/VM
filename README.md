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
qemu-system-x86_64 -enable-kvm -hda testvm.img -cdrom <path>/iso/ubuntu-20.10-desktop-amd64.iso -boot d -m 1024 
```
Start VM:
```bash
qemu-system-x86_64 -enable-kvm -hda testvm.img -m 3G
```

Tastenkombination von QEMU:
- Strg + Alt	: Maus aus dem QEMU-Fenster befreien
- Strg + Alt + 2	: vom Gast in den QEMU-Monitor wechseln
- Strg + Alt + 1	: vom QEMU-Monitor ins Gast-Betriebssystem wechseln
- Strg + Alt + F	: zwischen Fenster- und Vollbildmodus wechseln

QEMU Optionen:
- -hda Datei	: gibt das Image der primären Festplatte an. Weitere Platten können mit -hdb, -hdc und -hdd angegeben werden.
- -cdrom Datei : gibt das zu verwendende CD-Laufwerk an. Es kann ein Gerät wie /dev/cdrom oder eine Imagedatei angegeben werden
- -daemonize	: löst den QEMU-Prozess vom Terminal; das Terminal kann nach dem Start ohne Beeinträchtigung geschlossen werden
- -boot : Laufwerksbuchstabe	gibt an, von welchem Laufwerk gestartet werden soll. a steht für Diskette, c für Festplatte, d für CD-ROM und n für einen Netzwerk-Boot
- -m : Speichergröße	gibt den zu verwendenden Arbeitsspeicher in MiB an. Vorbereitung dazu s.o.
- -soundhw KARTE	es wird die Soundkarte KARTE emuliert; zur Auswahl stehen: sb16, es1370 und all
- -smp X	: es werden X CPU in der virtuellen Maschine genutzt, die Anzahl der virtuellen CPUs kann höher sein als die des realen Wirts
- -vnc :X	: Die Ausgabe des Bildschirms erfolgt per VNC auf Display X und nicht auf den normalen Bildschirm des Wirts, Details siehe auch hier
- -snapshot	:Dies bewirkt, dass Änderungen nicht in das Festplattenimage geschrieben, sondern in einer temporären Datei gespeichert werden. Erst mit den Tasten Strg + Alt + S oder dem Kommando commit in der QEMU-Konsole werden die Änderungen übernommen
- -hostfwd=[tcp|udp]:[Hostadresse]:Hostport-[Gastadresse]:Gastport	Leitet einen Port des Gasts auf einen Port des Hosts weiter. D.h. -hostfwd=::8008::80 macht einen Apache-Server (bei Standardkonfiguration) des Gastsystems unter http://localhost:8008 auf dem Wirt sichtbar. Oder -hostfwd=::8022::22 erlaubt ssh-Zugriff (bei Standardkonfiguration) auf das Gastsystem vom Wirt via ssh -p 8022 localhost
- -no-quit	Deaktiviert die Fenster-Schließen-Option. Verhindert z.B., dass beim Drücken von Alt + F4 das Fenster geschlossen und die VM beendet wird, wenn die Kombination eigentlich für den Gast gedacht war (z.B. dort Fenster schließen oder zur 4. Virtuellen Konsole wechseln)
- -cpu X	Legt den Typ der CPU fest, mittels -cpu host kann die Host-CPU definiert werden (funktioniert nur in Verbindung mit aktiviertem KVM: -enable-kvm)

## Download sources for ISOs

### Fedora
https://fedoraproject.org/de/workstation/download

### Ubuntu
https://ubuntu.com/download

### FreeBSD
https://www.freebsd.org/de/

### Windows10
https://www.microsoft.com/de-de/software-download/windows10ISO


