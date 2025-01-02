---
title: Konfigurace Správce Zavaděče
description: Nastavení správce zavaděče a předávání parametrů jádra do příkazového řádku
---

## systemd-boot

systemd-boot má dva druhy konfiguračních souborů, jeden pro samotný systemd-boot v `/boot/loader/loader.conf` a druhý pro každý jednotlivý záznam jádra v `/boot/loader/entry`.

### Konfigurace Loaderu

V tomto konfiguračním souboru můžete změnit výchozí záznam a časový limit systemd-boot.

```shell
# /boot/loader/loader.conf

default @saved
timeout 5
#console-mode keep # Tato možnost konfiguruje rozlišení konzole.
```

### Konfigurace Příkazového Řádku Jádra

Poskytujeme nástroj pro snadnější konfiguraci systemd-boot [`sdboot-manage`](https://github.com/CachyOS/CachyOS-PKGBUILDS/tree/master/systemd-boot-manager).
Jednou z výhod tohoto nástroje je globální konfigurace příkazového řádku jádra. Konfigurační soubor pro `sdboot-manage` se nachází v `/etc/sdboot-manage.conf`.
Pro změnu parametrů jádra upravte řádek `LINUX_OPTIONS=` v `/etc/sdboot-manage.conf`.

```shell
# /etc/sdboot-manage.conf
LINUX_OPTIONS="zswap.enabled=0 nowatchdog quiet splash"
```

Po provedení změn regenerujte všechny záznamy systemd-boot pomocí následujícího příkazu:

```shell
❯ sudo sdboot-manage gen
```

## rEFInd

Stejně jako [systemd-boot](/configuration/boot_manager_configuration#systemd-boot) má rEFInd dva konfigurační soubory. `refind.conf`, který se nachází v
`boot/efi/EFI/refind`, je určen hlavně pro změnu chování rEFInd, zatímco `/boot/refind_linux.conf` slouží ke správě vašich možností bootování.
Soubor `refind.conf` obsahuje rozsáhlé komentáře vysvětlující všechny jeho možnosti.

### Konfigurace Příkazového Řádku Jádra

Pro předání parametrů jádra do příkazového řádku upravte "Boot using default options" v `/boot/refind_linux.conf`.

```shell
# /boot/refind_linux.conf

"Boot using default options"     "root=PARTUUID=1cb353ec-7f03-4820-8b4b-03baf53a208f rw zswap.enabled=0 nowatchdog quiet splash"
```

Změny obou konfiguračních souborů se projeví okamžitě. Není nutné spouštět žádný příkaz pro „uložení“ změn.

## GRUB

Na rozdíl od [systemd-boot](/configuration/boot_manager_configuration#systemd-boot) a [rEFInd](/configuration/boot_manager_configuration#refind) má GRUB pouze jeden konfigurační soubor umístěný v `/etc/default/grub`. Tento soubor obsahuje velmi dobrou dokumentaci vysvětlující, co každá možnost znamená.

### Skrytí GRUB Boot Menu

Pro skrytí nabídky GRUB jednoduše nastavte následující možnosti:

```shell
# /etc/default/grub

GRUB_TIMEOUT='0'
GRUB_TIMEOUT_STYLE=hidden
```

Pro přístup k příkazovému řádku GRUB stiskněte ESC. Zde spusťte `normal` nebo `exit`, abyste se vrátili k obvyklé nabídce GRUB.

### Konfigurace Příkazového Řádku Jádra

Pro předání parametrů jádra do příkazového řádku s GRUB je potřeba upravit `GRUB_CMDLINE_LINUX_DEFAULT` v `/etc/default/grub`.

```shell
# /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT='nowatchdog zswap.enabled=0 quiet splash'
```

Pokaždé, když změníme konfigurační soubor GRUB, musíme znovu vytvořit konfiguraci pomocí následujícího příkazu:

```shell
❯ sudo grub-mkconfig -o /boot/grub/grub.cfg
```

## Další informace

- [Manuálová stránka loader.conf](https://man.archlinux.org/man/loader.conf.5)
- [rEFInd: Konfigurace správce zavaděče](https://www.rodsbooks.com/refind/configfile.html)
- [GRUB Manuál: Konfigurace](https://www.gnu.org/software/grub/manual/grub/grub.html#Configuration)
