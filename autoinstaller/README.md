# sysprep new iso

## for extract files 
```bash
xorriso -osirrox on -indev ../ubuntu-22.04.4-live-server-amd64.iso -extract_boot_images 2/bootpart -extract / 2
```
-osirrox --> extract boot image

```bash -->  make image
 xorriso -as mkisofs -r -V "custom-ubuntu2" -J -boot-load-size 4 -boot-info-table -input-charset utf-8 -eltorito-alt-boot -b bootpart/eltorito_img1_bios.img -no-emul-boot -o custom-lvm.iso .
```

-as mkisofs -->  Compatibility emulation (option list may be ended by list delimiter 


# autoinstall
```bash
autoinstall:
    version: 1
    ....
```
Subiquity searches for the autoinstall file in the following order and uses the first existing one:

1- Kernel command line
2- Root of the installation system
3- cloud-config
4- Root of the installation medium (ISO)

```bash
#cloud-config
autoinstall:
    version: 1
    reporting:
      central:
        type: rsyslog
        destination: "@192.168.0.1"
    interactive-sections:
      - network #stops on the network screen and allows the user to change the defaults. If a value is provided for an interactive section, it is used as the default. 
    identity:
      username: ubuntu
      password: $crypted_pass
    early-commands: #can not be interactive
        - ping -c1 172.16.50.
    locale: en_US
    network:
        network:
            version: 2
            ethernets:
                enp0s25:
                   dhcp4: no
                enp3s0: {}
                enp4s0: {}
            bonds:
                bond0:
                    dhcp4: yes
                    interfaces:
                        - enp3s0
                        - enp4s0

    proxy: http://squid.internal:3128/
    apt:
        primary:
            - arches: [default]
              uri: http://repo.internal/
        sources:
            my-ppa.list:
                source: "deb http://ppa.launchpad.net/curtin-dev/test-archive/ubuntu $RELEASE main"
    storage:
        layout:
            name: lvm
            sizing-policy: all #scaled: Adjust space allocated to the root logical volume (LV) based on space available to the volume group (VG). 
            # all: Allocate all remaining VG space to the root LV.

    identity:
        hostname: hostname
        username: username
        password: $crypted_pass
    ssh:
        install-server: yes
        authorized-keys:
          - $key
        allow-pw: no
    packages:
        - dns-server
    user-data: #can not be interactive
        disable_root: false
    late-commands: #can not be interactive
        - sed -ie 's/GRUB_TIMEOUT=.*/GRUB_TIMEOUT=30/' /target/etc/default/grub
    error-commands: #can not be interactive
        - tar c /var/log/installer | nc 192.168.0.1 100
        
```


subiquity: Ubuntu Server Installer, and backend for Ubuntu Desktop Installer


## cloud init

User-Data Script

begins with: "#!" or "Content-Type: text/x-shellscript"
script will be executed at "rc.local-like" level during first boot. rc.local-like means "very late in the boot sequence"

## Cloud Config Data

begins with "#cloud-config" or "Content-Type: text/cloud-config"
This content is "cloud-config" data. See the examples for a commented example of supported config formats.

