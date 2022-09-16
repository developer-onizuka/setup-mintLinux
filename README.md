# setup-mintLinux

# 0. Hardware
```
   (1) Precision Tower 3620
       Intel(R) Core(TM) i5-7600 CPU @ 3.50GHz
       DIMM slot1: DDR4 DIMM 16GB (KLEVV)
       DIMM slot2: DDR4 DIMM 16GB (KLEVV)
       DIMM slot3: DDR4 DIMM 16GB (KLEVV)
       DIMM slot4: DDR4 DIMM 16GB (KLEVV)
   (2) SATA SSD (SSD Type: MLC)
       TOSHIBA THNSNH128GCST 128GB
   (3) NVIDIA Quadro P1000
       For Pass Through at Virtual Machine (do not use as VGA but for GPGPU)
   (4) NVIDIA Quadro K600
       For VGA at Host Machine for Dual Display
   (5) Web Camera
       Logitech, Inc. Webcam C270
```
# 0. GPU Driver
https://ja.linuxcapable.com/how-to-install-nvidia-drivers-on-linux-mint-21-lts/

# 1. Chrome install on Host Machine mintLinux
```
$ sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
$ sudo wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
$ sudo apt update
$ sudo apt-get install -y google-chrome-stable
```

# 2. Enable IOMMU and Load vfio-pci driver instead of Nvidia driver on Host Machine
```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_iommu=on vfio_iommu_type1.allow_unsafe_interrupts=1 iommu=pt vfio-pci.ids=10de:1cb1,10de:0fb9"

$ sudo update-grub
$ sudo reboot
```
```
$ lspci -nnk -d 10de:1cb1
01:00.0 VGA compatible controller [0300]: NVIDIA Corporation GP107GL [Quadro P1000] [10de:1cb1] (rev a1)
	Subsystem: NVIDIA Corporation GP107GL [Quadro P1000] [10de:11bc]
	Kernel driver in use: vfio-pci
	Kernel modules: nvidiafb, nouveau
$ lspci -nnk -d 10de:0fb9
01:00.1 Audio device [0403]: NVIDIA Corporation GP107GL High Definition Audio Controller [10de:0fb9] (rev a1)
	Subsystem: NVIDIA Corporation GP107GL High Definition Audio Controller [10de:11bc]
	Kernel driver in use: vfio-pci
	Kernel modules: snd_hda_intel
```

# 3. Install KVM on Host Machine
```
$ sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
$ sudo apt install -y virt-manager
```

# 4. Install Vagrant
```
$ sudo apt install --yes vagrant vagrant-libvirt
$ sudo reboot
```

# 5. Run Vagrant and GPU workloads
You might use Vagrant file. See also https://github.com/developer-onizuka/nvidia-docker_VirtualMachine2 .
```
$ git clone https://github.com/developer-onizuka/nvidia-docker_VirtualMachine2
$ cd nvidia-docker_VirtualMachine2
$ vagrant up --provider=libvirt
```

