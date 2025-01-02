---
title: CachyOS chroot Helper
description: Nástroj pro usnadnění chrootování do systémů
---

[**`cachy-chroot`**](https://github.com/CachyOS/cachy-chroot) je jednoduchý pomocný program, který usnadňuje proces chrootování do existujících
instalací CachyOS nebo jakýchkoli instalací založených na Archu. Vypíše všechny oddíly nalezené na počítači a také podporuje výpis subvolumů BTRFS.
V neposlední řadě `cachy-chroot` podporuje také šifrované systémy pomocí LUKS. Namapuje každý záznam `fstab` na určené záznamy `crypttab`
a při ukončení chrootu elegantně uzavře všechny svazky LUKS.

## Použití

Proces chrootování **musí** být proveden na živém ISO. Níže je příklad použití `cachy-chroot` v instalaci CachyOS BTRFS.

```sh title="chrootování s cachy-chroot"
❯ sudo su # Přihlášení do uživatele root v živém ISO
❯ pacman -Sy cachy-chroot # Ujistěte se, že cachy-chroot je v nejnovější verzi
❯ cachy-chroot
Info: Found 3 block devices
Info: Found partition: Partition: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
Info: Found partition: Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Found partition: Partition: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
? Vyberte blokové zařízení pro kořenový oddíl (použijte šipky): ›
  Oddíl: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
❯ Oddíl: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
  Oddíl: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
✔ Vyberte blokové zařízení pro kořenový oddíl (použijte šipky): · Oddíl: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Vybrán oddíl BTRFS, připojuji a vypisuji subvolumy...
Info: Připojuji oddíl /dev/nvme0n1p2 do /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm s možnostmi: []
Info: Odpojuji oddíl v /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm
? Chcete použít předvolbu CachyOS BTRFS pro automatické připojení kořenového subvolumu? (y/n) › # Zadejte y, pokud jste na CachyOS
```

Po výběru kořenového oddílu program vyzve k připojení dalších oddílů, např. oddílu `/boot`

```sh title="Připojování dalších oddílů"
✔ Chcete připojit další oddíly? · ano
? Zadejte přípojný bod pro další oddíl (např. /boot), zadejte 'skip' pro zrušení: › # /boot pro systemd-boot, /boot/efi pro GRUB a rEFInd
```

Po dokončení ukončete prostředí chroot zadáním `exit` do výzvy nebo stisknutím `CTRL+D` na klávesnici.

```sh title="Ukončení chrootu"
exit
```

## Další informace

- [Arch Wiki - chroot](https://wiki.archlinux.org/title/Chroot)
