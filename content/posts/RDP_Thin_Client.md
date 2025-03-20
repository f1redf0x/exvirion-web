+++
title = 'Creating an RDP Thin Client with Debian'
date = 2025-03-20T11:41:34-05:00
draft = false
tags = ["thin-client", "digital minimalism"]
categories = ["Security"]
+++

# Introduction

When you first become a hacker, you develop something of a "hacker vision" that follows you when you go about. You tend to notice things like smart meters that control the city's electricity supply, or kiosks that are running intel compute sticks, or whatever. When you understand how the tech works, you can't help but ask yourself if you could exploit that.

And hospitals will drive you crazy.

Hospitals are highly digitized palaces with open ethernet jacks, presumed places of privacy, and computers running Windows 7 because their scanners aren't compatible with anything else. I wouldn't be surprised if every hospital is currently hacked.

But for all my pessimism, I noticed something about all of the "snatch-and-grabble computers" that they have around the standard medical offices. They are super tiny, under-spec'd, and look cheap. I found out the reason for this is for the longest time, those devices were hot-ticket theft items. A nurse or a doctor might leave a laptop on a cart in an inconvenient privacy spot away from the cameras. A thief could grab it, then use wifi from their car to access medical data, pilfer data, and who knows how you could misuse that laptop's access to medical databases with drugs and insurance payment sheets.

In short, this was a problem, and they needed a solution. 

Enter thin clients.

A thin client is a computer whose only job is to boot up into some full screen remote control program and grab a VDI or, virtual desktop infrastructure, machine from a server somewhere. 

If a thief steals a thin client, there is no data to pilfer on the hard-drive, and without proper creds, they couldn't get access to the remote server. Add in modern requirements for smart cards, and suddenly a thief has access to a machine that could be outperformed by a raspberry pi or a 100$ chromebook. If the network admin was clever, he would have made the gateway to the VDI only accessible through a VPN router on the hospital's network, so the thin client will just time out.

# The Project

So thin clients have been a thing forever, but I haven't been impressed by the linux offerings. You can install a linux operating system, install Remmina or some other RDP software, etc... but it just didn't have that nice ephemerality that comes with RAMFSes and that bothered me. 

I came across an [article](http://tinycorelinux.net/book.html) put out by the "tiny core" linux guys that explains how to create an ephemeral live drive with remote software that did just what I wanted. Unfortunately, I'm not taken with overwhelming confidence with the tiny core project. The website doesn't have any encryption, and when I booted tiny core on my laptop, it took a lot of finangling to get features online that were supposed to be "quick install".

Note: Please don't infer that I have anything less than huge respect for the tiny core team. Those guys and gals are hardcore enthusiast who've built an AWESOME project, it just wasn't for my use case. If I happen to have a Vocore board, I'll know who to use.

I decided that I would make a version of the live drive project from that book but with Debian and xfreerdp. At some point, I'm going to also add tailscale to it because I love everything about how tailscale does things, but that'll be a different time. The following is how you create a live USB stick with just enough software to get an ethernet attached thin client.

Note: I didn't even add wireless into it because like the hospital scenerio, I like the idea that a seperate device handles the connection and if someone takes the thin client machine it becomes a paperweight. Adding it wouldn't be hard. It would just take a couple more installs in the chroot stage.

## Instructions:

Keep in mind that I'm running an Ubuntu 24.04 machine as my base.

## Get the tools to bootstrap a debian instance onto your ubuntu machine.
```
sudo apt-get install \
    debootstrap \
    squashfs-tools \
    xorriso \
    isolinux \
    syslinux-efi \
    grub-pc-bin \
    grub-efi-amd64-bin \
    grub-efi-ia32-bin \
    mtools \
    dosfstools
```
## Create the boot folder to install everything to.
```
mkdir -p "${HOME}/LIVE_BOOT"
```
## Bootstrap and configure Debian.
```
sudo debootstrap \
    --arch=amd64 \
    --variant=minbase \
    stable \
    "${HOME}/LIVE_BOOT/chroot" \
    http://ftp.us.debian.org/debian/
```
## Choose your Hostname.
```
echo "debian-live" | sudo tee "${HOME}/LIVE_BOOT/chroot/etc/hostname"
```
## Install the image/kernel.
```
sudo chroot "${HOME}/LIVE_BOOT/chroot" /bin/bash << EOF
apt-get update && \
apt-get install -y --no-install-recommends \
    linux-image-amd64 \
    live-boot \
	 zsh \
    systemd-sysv
EOF
```

## Drop into chroot.

Note: Will drop you into the dev jail.

```
sudo chroot "${HOME}/LIVE_BOOT/chroot" 
```

## Install rdp tools and supporting OS goodies.
```
apt-get install -y --no-install-recommends \
    iwd \
    curl openssh-client \
    openbox xserver-xorg-core xserver-xorg xinit xterm lightdm tint2 freerdp2-x11\
    vim iproute2 net-tools isc-dhcp-client iputils-ping network-manager htop
```
## (STILL WORKING ON IT, not ready for primetime) Install tailscale.

```
# curl -fsSL https://tailscale.com/install.sh | sh
```

## Set up openbox.

```
mkdir -p ~/.config/openbox && cp /etc/xdg/openbox/* ~/.config/openbox
```

## Configure Openbox's Autostart and make sure to replace the variables.
```
vim ~/.config/openbox/autostart
```
add these lines:

```
# Launch taskbar
tint2 &
#Tailscale (uncomment next line when error is figured out)
#tailscale up --auth-key=INSERT_KEY_HERE &
# Start xfreerdp
xfreerdp /u:USERNAME /v:IP_ADDRESS /sound:sys:alsa /f /cert:ignore +clipboard &
```

## Configure lightdm.
```
vim /etc/lightdm/lightdm.conf
```
make sure these lines are there:

```
[Seat:*]
autologin-user=root
```
## Clean up uneeded packages.

```
apt-get clean
```
## Exit the chroot
```
exit
```
## Set the root password.

```
sudo chroot "${HOME}/LIVE_BOOT/chroot" passwd root
```

## Create Live environment files.
```
mkdir -p "${HOME}/LIVE_BOOT"/{staging/{EFI/BOOT,boot/grub/x86_64-efi,isolinux,live},tmp}
```
## Compress the chroot env into a squashfs.

```
sudo mksquashfs \
    "${HOME}/LIVE_BOOT/chroot" \
    "${HOME}/LIVE_BOOT/staging/live/filesystem.squashfs" \
    -e boot
```
## Copy the kernel and initramfs from inside the `chroot` to the `live` directory.
```
cp "${HOME}/LIVE_BOOT/chroot/boot"/vmlinuz-* \
    "${HOME}/LIVE_BOOT/staging/live/vmlinuz" && \
cp "${HOME}/LIVE_BOOT/chroot/boot"/initrd.img-* \
    "${HOME}/LIVE_BOOT/staging/live/initrd"
```
## Prepare Boot Loader Menus.
```
cat <<'EOF' > "${HOME}/LIVE_BOOT/staging/isolinux/isolinux.cfg"
UI vesamenu.c32

MENU TITLE Boot Menu
DEFAULT linux
TIMEOUT 600
MENU RESOLUTION 640 480
MENU COLOR border       30;44   #40ffffff #a0000000 std
MENU COLOR title        1;36;44 #9033ccff #a0000000 std
MENU COLOR sel          7;37;40 #e0ffffff #20ffffff all
MENU COLOR unsel        37;44   #50ffffff #a0000000 std
MENU COLOR help         37;40   #c0ffffff #a0000000 std
MENU COLOR timeout_msg  37;40   #80ffffff #00000000 std
MENU COLOR timeout      1;37;40 #c0ffffff #00000000 std
MENU COLOR msg07        37;40   #90ffffff #a0000000 std
MENU COLOR tabmsg       31;40   #30ffffff #00000000 std

LABEL linux
  MENU LABEL Debian Live [BIOS/ISOLINUX]
  MENU DEFAULT
  KERNEL /live/vmlinuz
  APPEND initrd=/live/initrd boot=live

LABEL linux
  MENU LABEL Debian Live [BIOS/ISOLINUX] (nomodeset)
  MENU DEFAULT
  KERNEL /live/vmlinuz
  APPEND initrd=/live/initrd boot=live nomodeset
EOF
```
## Create Grub menu.
```
cat <<'EOF' > "${HOME}/LIVE_BOOT/staging/boot/grub/grub.cfg"
insmod part_gpt
insmod part_msdos
insmod fat
insmod iso9660

insmod all_video
insmod font

set default="0"
set timeout=30

# If X has issues finding screens, experiment with/without nomodeset.

menuentry "Debian Live [EFI/GRUB]" {
    search --no-floppy --set=root --label DEBLIVE
    linux ($root)/live/vmlinuz boot=live
    initrd ($root)/live/initrd
}

menuentry "Debian Live [EFI/GRUB] (nomodeset)" {
    search --no-floppy --set=root --label DEBLIVE
    linux ($root)/live/vmlinuz boot=live nomodeset
    initrd ($root)/live/initrd
}
EOF
```
## Copy grub to boot.
```
cp "${HOME}/LIVE_BOOT/staging/boot/grub/grub.cfg" "${HOME}/LIVE_BOOT/staging/EFI/BOOT/"
```
## Create grub-embed.cfg.
```
cat <<'EOF' > "${HOME}/LIVE_BOOT/tmp/grub-embed.cfg"
if ! [ -d "$cmdpath" ]; then
    # On some firmware, GRUB has a wrong cmdpath when booted from an optical disc.
    # https://gitlab.archlinux.org/archlinux/archiso/-/issues/183
    if regexp --set=1:isodevice '^(\([^)]+\))\/?[Ee][Ff][Ii]\/[Bb][Oo][Oo][Tt]\/?$' "$cmdpath"; then
        cmdpath="${isodevice}/EFI/BOOT"
    fi
fi
configfile "${cmdpath}/grub.cfg"
EOF
```
## Prepare boot loader files:
```
cp /usr/lib/ISOLINUX/isolinux.bin "${HOME}/LIVE_BOOT/staging/isolinux/" && \
cp /usr/lib/syslinux/modules/bios/* "${HOME}/LIVE_BOOT/staging/isolinux/"
```
## Copy EFI/Modern boot files.
```
cp -r /usr/lib/grub/x86_64-efi/* "${HOME}/LIVE_BOOT/staging/boot/grub/x86_64-efi/"
```
## Create EFI bootable Grub images.
```
grub-mkstandalone -O i386-efi \
    --modules="part_gpt part_msdos fat iso9660" \
    --locales="" \
    --themes="" \
    --fonts="" \
    --output="${HOME}/LIVE_BOOT/staging/EFI/BOOT/BOOTIA32.EFI" \
    "boot/grub/grub.cfg=${HOME}/LIVE_BOOT/tmp/grub-embed.cfg"
```
## Create Fat16 UEFI Boot disk image with EFI Bootloaders.
```
(cd "${HOME}/LIVE_BOOT/staging" && \
    dd if=/dev/zero of=efiboot.img bs=1M count=20 && \
    mkfs.vfat efiboot.img && \
    mmd -i efiboot.img ::/EFI ::/EFI/BOOT && \
    mcopy -vi efiboot.img \
        "${HOME}/LIVE_BOOT/staging/EFI/BOOT/BOOTIA32.EFI" \
        "${HOME}/LIVE_BOOT/staging/EFI/BOOT/BOOTx64.EFI" \
        "${HOME}/LIVE_BOOT/staging/boot/grub/grub.cfg" \
        ::/EFI/BOOT/
)
```
## Use xorriso to generate iso.

```
xorriso \
    -as mkisofs \
    -iso-level 3 \
    -o "${HOME}/LIVE_BOOT/debian-custom.iso" \
    -full-iso9660-filenames \
    -volid "DEBLIVE" \
    --mbr-force-bootable -partition_offset 16 \
    -joliet -joliet-long -rational-rock \
    -isohybrid-mbr /usr/lib/ISOLINUX/isohdpfx.bin \
    -eltorito-boot \
        isolinux/isolinux.bin \
        -no-emul-boot \
        -boot-load-size 4 \
        -boot-info-table \
        --eltorito-catalog isolinux/isolinux.cat \
    -eltorito-alt-boot \
        -e --interval:appended_partition_2:all:: \
        -no-emul-boot \
        -isohybrid-gpt-basdat \
    -append_partition 2 C12A7328-F81F-11D2-BA4B-00A0C93EC93B ${HOME}/LIVE_BOOT/staging/efiboot.img \
    "${HOME}/LIVE_BOOT/staging"
```
Ok, so at this point you have an iso file that'll launch a minimal version of debian, set up xfreerdp on fullscreen to point to your server that's running RDP that you want to act as your virtual infrastructure. 

Transfer that ISO to a USB stick using etcher, dd, or my personal favorite: gnome-disks.

Now as far as the VDI that you're connecting to is concerned, it could be a windows virtual machine or something else. Personally, I prefer debian-based linux systems, so let's set up an RDP server on another debian instance. I'm assuming you installed it clean and didn't add any software yet, so it's just a terminal.

---

# Installing an RDP Server on Debian

## Update apt.

```
sudo apt update && sudo apt upgrade -y
```
## Install a gui server and a desktop environment.
```
sudo apt install xfce4 xfce4-goodies xorg dbus-x11 x11-xserver-utils -y
```
## Install xrdp (The server program).
```
sudo apt install xrdp -y
```
## Make sure it's running.
```
sudo systemctl status xrdp
```
## let the program use SSL so there's a layer of encryption.
```
sudo adduser xrdp ssl-cert
```
## Configure your debian instance to use the encryption files.
```
vim /etc/xrdp/xrdp.ini
```

change these:

```
security_layer=negotiate
certificate=
key_file=
```

To these:

```
security_layer=tls
certificate=/etc/xrdp/cert.pem
key_file=/etc/xrdp/key.pem
```
## Restart the xRDP service.

```
sudo systemctl restart xrdp
```

## If you have a firewall, let it through. Debian doesn't have one by default.
```
sudo ufw allow 3389/tcp
```
## If you have a firewall, reload it.
```
sudo ufw reload
```

## A Note about RDP failure

There was a feature that I wasn't aware of with RDP. Only one person can be RDP'ing into the server at one time. 

If you set up xrdp and you try to log in with xfreerdp and you get a message like "user is being logged out", it's because it's trying to take ownership of the one graphical shell that's being offered, but It's already being taken.

Try logging off on the server and give it a go.

# Where all this comes from:

Taken almost directly from: https://www.willhaley.com/blog/custom-debian-live-environment/

RDP: https://www.aguslr.com/blog/2017/04/15/debian-thinclient.html

RDP Server: https://itslinuxfoss.com/install-xrdp-server-remote-desktop-debian-12/

Xfreerdp: https://commandmasters.com/commands/xfreerdp-linux/


