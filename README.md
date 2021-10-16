# setup-mintLinux

# 0. Hardware
```
   (1) Precision Tower 3620
       Intel(R) Core(TM) i5-7600 CPU @ 3.50GHz
       DIMM slot1: DDR4 DIMM 16GB (KLEVV)
       DIMM slot2: DDR4 DIMM 16GB (KLEVV)
       DIMM slot3: Empty
       DIMM slot4: Empty
   (2) NVMe SSD
       KLEVV SSD 256GB CRAS C710 M.2 Type2280 PCIe3x4 NVMe 3D TLC NAND Flash
       P/N: K256GM2SP0-C71
       Performance Spec: Read 1950MB/s, Write 1250MB/s
   (3) NVIDIA Quadro P1000
       For Pass Through at Virtual Machine (do not use as VGA but for GPGPU)
   (4) NVIDIA Quadro K600
       For VGA at Host Machine for Dual Display
   (5) Web Camera
       Logitech, Inc. Webcam C270
```

# 1. Chrome install on Host Machine mintLinux
```
$ sudo apt-get install -y google-chrome-stable
```

# 2. Enable IOMMU and Load vfio-pci driver instead of Nvidia driver on Host Machine
```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash intel_iommu=on vfio_iommu_type1.allow_unsafe_interrupts=1 iommu=pt vfio-pci.ids=10de:1cb1,10de:0fb9"

$ sudo update-grub

$ lspci -nnk -d 10de:1cb1
$ lspci -nnk -d 10de:0fb9
```

