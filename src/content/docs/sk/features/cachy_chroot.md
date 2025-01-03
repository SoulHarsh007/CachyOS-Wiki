---
title: Pomocník CachyOS chroot
description: Pomocný nástroj na uľahčenie vstupu do chroot v systémoch
---

[**`cachy-chroot`**](https://github.com/CachyOS/cachy-chroot) je jednoduchý pomocný program na uľahčenie procesu chrootovania do existujúcej
inštalácie CachyOS alebo akejkoľvek Arch-based inštalácie. Zobrazuje zoznam všetkých oddielov nájdených na počítači a tiež podporuje zobrazovanie podzväzkov BTRFS.
V neposlednom rade `cachy-chroot` podporuje aj šifrované systémy prostredníctvom LUKS. Priradí každú položku `fstab` k jej prislúchajúcim položkám `crypttab`
a pri opustení chrootu elegantne zatvorí všetky zväzky LUKS.

## Použitie

Proces chrootovania sa **musí** vykonať na live ISO. Nižšie je uvedený príklad použitia `cachy-chroot` v inštalácii CachyOS BTRFS.

```sh title="chrootovanie pomocou cachy-chroot"
❯ sudo su # Vstup do používateľa root v rámci live ISO
❯ pacman -Sy cachy-chroot # Uistite sa, že cachy-chroot je v najnovšej verzii
❯ cachy-chroot
Info: Nájdené 3 blokové zariadenia
Info: Nájdený oddiel: Partition: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
Info: Nájdený oddiel: Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Nájdený oddiel: Partition: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
? Vyberte blokové zariadenie pre koreňový oddiel (použite šípky):  ›
  Partition: /dev/nvme0n1p1: FS: vfat UUID: EDA6-ED98
❯ Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
  Partition: /dev/nvme0n1p4: FS: btrfs UUID: 66e84339-8c77-4131-afce-50ec2cf67a80
✔ Vyberte blokové zariadenie pre koreňový oddiel (použite šípky):  · Partition: /dev/nvme0n1p2: FS: btrfs UUID: b09a027e-a61d-424f-858f-2e02be61b342
Info: Vybraný BTRFS oddiel, pripájam a zobrazujem zoznam podzväzkov...
Info: Pripájam oddiel /dev/nvme0n1p2 na /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm s možnosťami: []
Info: Odpájam oddiel na /tmp/cachyos-chroot-temp-mount-b09a027e-a61d-424f-858f-2e02be61b342-hwAeIm
? Chcete použiť predvoľbu CachyOS BTRFS na automatické pripojenie koreňového podzväzku? (y/n) › # Zadajte y, ak ste na CachyOS
```

Po výbere koreňového oddielu program vyzve na pripojenie ďalších oddielov, napr. oddielu `/boot`.

```sh title="Pripájanie ďalších oddielov"
✔ Chcete pripojiť ďalšie oddiely? · yes
? Zadajte pripojovací bod pre ďalší oddiel (napr. /boot) zadajte 'skip' pre zrušenie:  › # /boot na systemd-boot, /boot/efi na GRUB a rEFInd
```

Po dokončení opustite chroot prostredie zadaním `exit` do výzvy alebo stlačením `CTRL+D` na klávesnici.

```sh title="Ukončenie chroot"
exit
```

## Zistite viac

- [Arch Wiki - chroot](https://wiki.archlinux.org/title/Chroot)
