# Debian Network Reinstall Script

[General description in English ↓](#introduction)

## VPS Network Reinstall Debian 11 Script

** Oracle Linux is not supported as the original system at this time. Please select the Ubuntu 20.04 or 18.04 system template when creating a new machine. **

To download the script:

```
curl -fLO https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh && chmod a+rx debi.sh
```

Run the script:

```
sudo . /debi.sh --cdn --network-console --ethx --bbr --user root --password <new system user password>
```

* ``-bbr`` Turn on BBR
* `--ethx` Use the traditional form of the NIC name, such as `eth0` instead of `ens3`
* `-cloud-kernel` installs the smaller `cloud` kernel, but may cause VNC blackouts on UEFI booted machines (e.g. Oracle, Azure and Hyper-V, Google Cloud, etc.) Regular VPS booted on BIOS do not have this problem.
* The default timezone is UTC, add `-timezone Asia/Shanghai` to use the Chinese timezone.
* Default is Debian official CDN mirror source (deb.debian.org), add `--china` to use Aliyun mirror source.

If no errors are reported you can restart:

```
sudo shutdown -r now
```

After about 30 seconds you can try SSH login to the `-installer` user with the same password as previously set. If the connection is successful, you can press Ctrl-A and then 4 to monitor the installation log. The installation will automatically reboot into the new system after completion.


### [Oracle automatically obtains IPv6](https://github.com/bohanyang/debi/wiki/%E7%94%B2%E9%AA%A8%E6%96%87%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%87% AA%E5%8A%A8%E8%8E%B7%E5%8F%96-IPv6)
### [How to install Oracle on a pure IPv6 network (no public IPv4)](https://github.com/bohanyang/debi/wiki/%E7%94%B2%E9%AA%A8%E6%96%87%E4%BA%91%E6%9C%8D%E5%8A%A1%E5% 99%A8%E7%BA%AF-IPv6-%E7%BD%91%E7%BB%9C%EF%BC%88%E6%97%A0%E5%85%AC%E7%BD%91-IPv4%EF%BC%89%E4%B8%8B%E5%AE%89%E8%A3%85%E6%96%B9%E6% B3%95)

## Introduction

This script is written to reinstall a VPS/virtual machine to minimal Debian 11.

## Should Work On

### Virtualization Platform

 * SolusVM/OpenStack/DigitalOcean/Vultr/Linode/Proxmox/QEMU KVM (BIOS boot)
 * Oracle Cloud Infrastructure (UEFI boot)
 * Google Cloud Compute Engine (**Must manually configure the VPC internal IP and the gateway.** UEFI boot with Secure Boot support)
 * AWS EC2 & Lightsail (BIOS boot)
 * Hyper-V & Azure (Generation 1 BIOS boot & Generation 2 UEFI boot)

### Original OS

 * Debian 8 or later
 * Ubuntu 14.04 or later
 * CentOS 7 or later

## How It Works

1. Generate a preseed file to automate installation
2. Download the 'Debian-Installer' to the `/boot` directory
Append a menu entry of the installer to the GRUB2 configuration file

## Usage

### 1. Download

Download the script with curl.

    curl -fLO https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh
    
    ## for IPv6-only machines
    curl -fLO --resolve 'raw.githubusercontent.com:443:2a04:4e42::133' https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh

or wget.

    wget -O debi.sh https://raw.githubusercontent.com/bohanyang/debi/master/debi.sh

### 2. Run

Run the script under root or using sudo.

    chmod a+rx debi.sh
    sudo . /debi.sh

By default, an admin user `debian` with sudo privilege will be created during the installation. Use `--user root` if you prefer.

### 3. Reboot

If everything looks good, reboot the machine.

    sudo shutdown -r now

Otherwise, you can run this command to revert all changes made by the script.

    sudo rm -rf debi.sh /etc/default/grub.d/zz-debi.cfg /boot/debian-* && { sudo update-grub || sudo grub2-mkconfig -o /boot/grub2/grub.cfg. 

Translated with www.DeepL.com/Translator (free version)
