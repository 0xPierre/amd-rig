# Rig Configuration for Hashcat

Hardware config :
- Intel(R) Core(TM) i9-11900KF @ 3.50GHz
- 8 x Radeon RX Vega 56/64

## Ubuntu 20.04 installation

First install a minimal Ubuntu 20.04, then follow the instructions below.

Add yourself to the required group, updates and install required packages.
```bash
sudo usermod -a -G render $LOGNAME
sudo apt-get update
sudo apt-get install wget gnupg2 gawk curl p7zip-full
```

Install the ROCm repository
```bash
sudo apt-get update
wget https://repo.radeon.com/amdgpu-install/5.4.2/ubuntu/focal/amdgpu-install_5.4.50402-1_all.deb
sudo apt-get install ./amdgpu-install_5.4.50402-1_all.deb
```

Install ROCm package
```bash
sudo amdgpu-install --usecase=rocm
```

Reboot the system
```bash
sudo reboot
```

Post-install Actions 
``` bash
export LD_LIBRARY_PATH=/opt/rocm-5.4.2/lib:/opt/rocm-5.4.2/lib64
export PATH=$PATH:/opt/rocm-5.4.2/bin:/opt/rocm-5.4.2/opencl/bin
```

Verify the installations. You should see the GPUs.
```
rocminfo
opencl/bin/clinfo
rocm-smi
```

Installation of Hashcat.
```bash
wget https://hashcat.net/files/hashcat-7.1.2.7z
7z x hashcat-7.1.2.7z
sudo mv hashcat-7.1.2 /opt
export PATH=$PATH:/opt/hashcat-7.1.2
```

Benchmark Hashcat
```bash
hashcat -b
```



## Sources
- https://archive.is/20240121234333/https://medium.com/@ttio2tech_28094/rocm-v5-4-2-installation-guide-for-amd-gpu-in-early-2023-8f5b3933d1d6#selection-560.36-560.37
