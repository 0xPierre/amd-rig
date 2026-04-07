# Rig Configuration for Hashcat

Hardware config :
- Intel(R) Core(TM) i9-11900KF @ 3.50GHz
- 8 x Radeon RX Vega 56/64

## Ubuntu 20.04 installation

First install a minimal Ubuntu 20.04, then follow the instructions below.

Add yourself to the required group, fix permissions, updates and install required packages.
```bash
sudo usermod -a -G render $LOGNAME
sudo chmod 666 /dev/dri/card0
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
clinfo
rocm-smi
```

Installation of Hashcat.
```bash
wget https://hashcat.net/files/hashcat-7.1.2.7z
7z x hashcat-7.1.2.7z
sudo mv hashcat-7.1.2 /opt
mv /opt/hashcat-7.1.2/hashcat.bin /opt/hashcat-7.1.2/hashcat
echo 'export PATH="$PATH:/opt/hashcat-7.1.2"' >> ~/.bashrc
```

Benchmark Hashcat
```bash
hashcat -b
```

## Simple Cracking Demo

#### Password Cracking with wordlist
Passwords to crack:

in hash.txt:
```
dcba8a186c68633dba69644277285466:babygirl10
7ab0171aec27a57da13621a615910e43:slipknot666
```
The cli:
```bash
hashcat hash.txt -m 0 -a 0 /usr/share/wordlists/rockyou.txt
```

#### Custom password bruteforce
For 7 chars it should takes max 30s
in hash2.txt
```
2592fe5f6de3412d66ecd88ee1f8757d:es1e442
```

The cli:
```bash
hashcat -a 3 -m 0 hash2.txt
```

Then
```bash
hashcat -a 3 -m 0 hash2.txt --show
```

## Sources
- https://archive.is/20240121234333/https://medium.com/@ttio2tech_28094/rocm-v5-4-2-installation-guide-for-amd-gpu-in-early-2023-8f5b3933d1d6#selection-560.36-560.37
