# Ubuntu
Installed in NEC 

The message `System BootOrder not found. Initializing defaults.` comes every time booting.

It is solved by below

```sh
$ sudo su
# cd /boot/efi/EFI
# mv BOOT BOOT_back
# cp -R ubuntu BOOT
# cd BOOT
# mv shimx64.efi bootx64.efi
```

need to study these commands.
