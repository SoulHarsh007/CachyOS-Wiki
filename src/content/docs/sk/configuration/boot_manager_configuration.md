---
title: Konfigurácia správcu zavádzania
description: Nakonfigurujte nastavenia správcu zavádzania a odovzdajte parametre jadra do príkazového riadku
---

## systemd-boot

systemd-boot má dva druhy konfiguračných súborov, jeden pre samotný systemd-boot v `/boot/loader/loader.conf` a jeden pre každú
jednotlivú položku jadra v `/boot/loader/entry`.

### Konfigurácia zavádzača

V tomto konfiguračnom súbore môžete zmeniť predvolenú položku a časový limit systemd-boot

```shell
# /boot/loader/loader.conf

default @saved
timeout 5
#console-mode keep # Táto možnosť konfiguruje rozlíšenie konzoly.
```

### Konfigurácia príkazového riadku jadra

Poskytujeme nástroj na jednoduchšiu konfiguráciu systemd-boot [`sdboot-manage`](https://github.com/CachyOS/CachyOS-PKGBUILDS/tree/master/systemd-boot-manager).
Jednou z výhod tohto nástroja je globálna konfigurácia príkazového riadku jadra. Konfiguračný súbor pre `sdboot-manage` sa nachádza v `/etc/sdboot-manage.conf`.
Upravte riadok `LINUX_OPTIONS=` v `/etc/sdboot-manage.conf` na zmenu parametrov jadra.

```shell
# /etc/sdboot-manage.conf
LINUX_OPTIONS="zswap.enabled=0 nowatchdog quiet splash"
```

Po vykonaní zmien, znova vygenerujte všetky položky systemd-boot pomocou nasledujúceho príkazu

```shell
❯ sudo sdboot-manage gen
```

## rEFInd

Podobne ako [systemd-boot](/configuration/boot_manager_configuration#systemd-boot), aj rEFInd má dva konfiguračné súbory. `refind.conf` sa nachádza v
`boot/efi/EFI/refind` a slúži hlavne na zmenu správania rEFInd, zatiaľ čo `/boot/refind_linux.conf` slúži na správu vašich možností spustenia.
`refind.conf` obsahuje rozsiahle komentáre, ktoré vysvetľujú všetky jeho možnosti.

### Konfigurácia príkazového riadku jadra

Ak chcete odovzdať parametre jadra do príkazového riadku, upravte "Boot using default options" v `/boot/refind_linux.conf`

```shell
# /boot/refind_linux.conf

"Boot using default options"     "root=PARTUUID=1cb353ec-7f03-4820-8b4b-03baf53a208f rw zswap.enabled=0 nowatchdog quiet splash"
```

Zmeny v oboch konfiguračných súboroch sa prejavia okamžite. Spúšťanie príkazu na "uloženie" zmien nie je potrebné.

## GRUB

Na rozdiel od [systemd-boot](/configuration/boot_manager_configuration#systemd-boot) a [rEFInd](/configuration/boot_manager_configuration#refind),
GRUB má iba jeden konfiguračný súbor, ktorý sa nachádza v `/etc/default/grub`. V tomto súbore je pomerne dobrá dokumentácia, ktorá vysvetľuje, čo
každá možnosť robí.

### Skrytie ponuky zavádzania GRUB

Ak chcete skryť ponuku GRUB, jednoducho nastavte nasledujúce možnosti zodpovedajúcim spôsobom.

```shell
# /etc/default/grub

GRUB_TIMEOUT='0'
GRUB_TIMEOUT_STYLE=hidden
```

Stlačením klávesy ESC získate prístup k riadku GRUB. Odtiaľto spustite `normal` alebo `exit`, aby ste sa vrátili do známej ponuky zavádzania GRUB.

### Konfigurácia príkazového riadku jadra

Ak chcete odovzdať parametre jadra do príkazového riadku pomocou GRUB, musíme upraviť `GRUB_CMDLINE_LINUX_DEFAULT` v rámci `/etc/default/grub`

```shell
# /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT='nowatchdog zswap.enabled=0 quiet splash'
```

Zakaždým, keď upravíme konfiguračný súbor GRUB, musíme znova vytvoriť konfiguráciu pomocou nasledujúceho príkazu

```shell
❯ sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Zistite viac

- [Manuálová stránka loader.conf](https://man.archlinux.org/man/loader.conf.5)
- [rEFInd: Konfigurácia správcu zavádzania](https://www.rodsbooks.com/refind/configfile.html)
- [Manuál GRUB: Konfigurácia](https://www.gnu.org/software/grub/manual/grub/grub.html#Configuration)
