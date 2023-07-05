# Create Kickstart Unattended Installation Redhat Linux / Rocky Linux ISO


wget https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.1-x86_64-dvd.iso

mount /dev/sr0 /mnt

mount -o loop /tmp/Rocky-9.1-x86_64-dvd.iso /mnt

tar cf - -C /mnt . | tar xf - -C /tmp/rocky9

ls -a /tmp/rocky9

cd /tmp/rocky9

vim /tmp/ks.cfg 

vim /tmp/rocky9/isolinux/isolinux.cfg  

label kickstart
  menu label ^Kicskstart Install Rocky Linux 9.1
  kernel vmlinuz
  append initrd=initrd.img inst.stage2=hd:LABEL=Rocky-9-1-x86_64-dvd quiet inst.ks=cdrom:/ks.cfg


vim /tmp/rocky9/EFI/BOOT/grub.cfg  

menuentry 'Kickstart Install Rocky Linux 9.1' --class fedora --class gnu-linux --class gnu --class os {
        linuxefi /images/pxeboot/vmlinuz inst.stage2=hd:LABEL=Rocky-9-1-x86_64-dvd quiet inst.ks=cdrom:/ks.cfg
        initrdefi /images/pxeboot/initrd.img
}

mkisofs -o /tmp/rocky9m.iso -b isolinux/isolinux.bin -J -R -l -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -eltorito-alt-boot -e images/efiboot.img -no-emul-boot -graft-points -joliet-long -V "Rocky-9-1-x86_64-dvd" .

isohybrid --uefi /tmp/rocky9m.iso

implantisomd5 /tmp/rocky9m.iso

dnf provides implantisomd5

dnf install syslinux isomd5sum xorriso

How to create a modified Red Hat Enterprise Linux ISO with kickstart file or modified installation media?  
https://access.redhat.com/solutions/60959

Kickstart Generator  
https://access.redhat.com/labs/kickstartconfig/

RHEL/CentOS 8 Kickstart example | Kickstart Generator  
https://www.golinuxcloud.com/rhel-centos-8-kickstart-example-generator/#Kickstart_command_-_url
