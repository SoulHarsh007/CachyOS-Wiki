---
title: Automatické připojování dalších disků při spuštění pomocí fstab
description: Připojujte další statické disky při spuštění pomocí souboru umístěného v /etc/fstab
---

Tento návod popisuje základy používání souboru fstab, který se nachází v /etc/, pro připojování statických disků během spouštění. Stručně vysvětlí, jak najít UUID oddílu nebo disku, co dělají některé možnosti a kde najít další informace, pokud by poskytnuté informace byly nedostatečné.

## Předpoklady
- Přístup root

## Přidávání záznamů do /etc/fstab

### 1. Zobrazení UUID vašich oddílů
V terminálu dle vašeho výběru (Konsole, Alacritty, Kitty atd.) spusťte následující příkaz:

```sh
❯ lsblk -f
NAME        FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
zram0                                                                              [SWAP]
nvme0n1
├─nvme0n1p1 vfat   FAT32       E04D-9F05
├─nvme0n1p2
├─nvme0n1p3 ntfs               08A24E90A24E81E4                      715.4G    50%
├─nvme0n1p4 vfat   FAT32       E09C-D4DA                             628.1M    39% /boot
├─nvme0n1p5 ext4   1.0         187a9f06-9411-48d9-b941-f03c2e605812  203.6G    47% /
└─nvme0n1p6 ntfs
```

V našem příkladu víme, že chceme připojit oddíl Windows, který je typu ntfs, a víme, že zhruba polovina jeho prostoru je volná. Můžeme tedy určit, že oddíl, který chceme připojit, je `nvme0n1p3` a jeho UUID je `08A24E90A24E81E4`, s typem souborového systému `ntfs` v tomto příkladu.

### 2. Identifikace vašeho oddílu

Často `lsblk -f` poskytne všechny informace, které potřebujete k připojení disku pomocí /etc/fstab. Pokud vám však informace chybí, můžete spustit následující příkaz:

```sh
❯ sudo fdisk -l
Disk /dev/nvme0n1: 1.86 TiB, 2000398934016 bytes, 3907029168 sectors
Disk model: WD_BLACK SN850X 2TB
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 80008000-0000-0000-0000-000000000000

Device              Start        End    Sectors  Size Type
/dev/nvme0n1p1       2048     206847     204800  100M EFI System
/dev/nvme0n1p2     206848     239615      32768   16M Microsoft reserved
/dev/nvme0n1p3     239616 2997384182 2997144567  1.4T Microsoft basic data
/dev/nvme0n1p4 2997385216 2999482367    2097152    1G EFI System
/dev/nvme0n1p5 2999482368 3905454079  905971712  432G Linux root (x86-64)
/dev/nvme0n1p6 3905454080 3907026943    1572864  768M Windows recovery environment
```

V tomto příkladu již známe naše UUID, nicméně `fdisk -l` nám to může trochu více objasnit zobrazením přesné velikosti oddílu (1.4T) a jeho typu (Microsoft basic data).

To by nám mělo jasně ukázat, že oddíl, který chceme, je `nvme0n1p3` s UUID `08A24E90A24E81E4`, jak bylo popsáno dříve. Věděli jsme to už dříve, ale teď si tím můžeme být jisti.

Jakmile si budete jisti, že jste našli správný oddíl, zkopírujte UUID. Kopírování z terminálu se obvykle provádí pomocí `ctrl+shift+C`.

### 3. Přidání záznamu do /etc/fstab

Nyní, když jsme získali UUID našeho oddílu, je čas otevřít soubor fstab.

Můžete použít textový editor dle vašeho výběru, v tomto příkladu použijeme nano. Pro úpravu souboru fstab je nutné jej otevřít jako root:

```sh
❯ sudo nano /etc/fstab
```

Pomocí šipek se přesuňte na konec souboru fstab a na nový řádek vytvoříme náš nový záznam:

```sh
UUID=08A24E90A24E81E4 /media/windows ntfs3 defaults,nofail 0 0
```
Rozpis tohoto záznamu je následující:

- `UUID=08A24E90A24E81E4` Toto je souborový systém, který chceme připojit, identifikovaný jeho UUID. Existují i jiné metody pro identifikaci souborového systému, i když UUID bývá nejbezpečnější. Další metody jsou uvedeny [zde](https://wiki.archlinux.org/title/Fstab#Identifying_file_systems).

- `/media/windows` [Standard hierarchie souborového systému Linux](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) říká, že `/media/` je správné umístění pro připojení vyměnitelných disků. `windows` označuje adresář, do kterého chceme náš disk připojit. Každý disk, který chceme připojit, bude potřebovat svůj vlastní adresář.

- `ntfs3` Toto je typ souborového systému pro náš souborový systém. V našem příkladu explicitně používáme ovladač jádra ntfs3. Dalšími příklady by byly `ext4`, `xfs` nebo podobné. Toto explicitní prohlášení o typu souborového systému lze nahradit `auto`, což umožní příkazu mount nejlépe odhadnout typ souborového systému.

- `defaults,nofail` Možnosti, které chceme předat příkazu mount pro tento disk. `nofail` znamená, že pokud se tento disk nepodaří připojit, nezpůsobí to chybu při spouštění. Spouštění bude pokračovat normálně. `defaults` implikuje standardní sadu logických možností. Typicky `rw`, `ro` nebo podobné.

- `první 0` dump, toto je v moderních systémech obvykle zastaralé. Ponechání této hodnoty na 0 nic nezpůsobí. Více si o tom můžete přečíst [zde](https://linux.die.net/man/8/dump).

- `druhá 0` Nastavuje pořadí kontroly souborového systému při spouštění. Pro kořenový oddíl (pokud váš kořenový souborový systém není btrfs nebo xfs, které by měly být nastaveny na 0) by to mělo být 1. Všechny ostatní souborové systémy ve vašem fstab by měly být buď 0 (zakázáno) nebo 2. Více informací [zde](https://man.archlinux.org/man/fsck.8).

Možnosti jsou podrobněji vysvětleny [zde](https://man7.org/linux/man-pages/man5/fstab.5.html) a [zde](https://man7.org/linux/man-pages/man8/mount.8.html).

#### Více informací
Mimochodem, všechny možnosti za deklarací typu souborového systému jsou volitelné, pokud je neměníte z výchozí hodnoty.

Tedy

`UUID=<UUID oddílu> /media/foo somefs`

a

`UUID=<UUID oddílu> /media/foo somefs defaults 0 0`

jsou ekvivalentní. `somefs` následované ničím je implicitně `somefs defaults 0 0`.

#### Důležité pro oddíly Windows

Pokud se řídíte tímto návodem s oddílem Windows, vaše možnosti by měly být `uid=1000,gid=1000,rw,user,exec,umask=000`, přičemž uid a gid nahradíte ID uživatele a ID skupiny. Pokud neudělíte oprávnění user a exec, Windows může zamknout váš disk a znemožnit vám cokoli upravovat. K tomu může dojít bez ohledu na oprávnění, pokud nevypnete rychlé spuštění.

Pokud nenastavíte umask=000, některé soubory mohou být nezapisovatelné v závislosti na nastavení.

### 4. Dokončení

Pokud chcete nyní připojit disk, pro který jste vytvořili záznam, musíte spustit následující příkaz:

```sh
❯ sudo systemctl daemon-reload
```

a poté:

```sh
❯ sudo mount -a
```

Váš disk by se nyní měl objevit pod `/media/windows` a objeví se tam i při příštím spuštění a i v budoucnu.

```sh
❯ ls /media/windows
'$Recycle.Bin'             Linux                  SteamLibrary
 AMD                       Modding                swapfile.sys
 Apps                      pagefile.sys          'System Volume Information'
 bootTel.dat               PerfLogs               Users
 Development               ProgramData            WiiU
'Documents and Settings'  'Program Files'         Windows
 DumpStack.log.tmp        'Program Files (x86)'   XboxGames
 FanControl                Recovery               xiv_modding
 Games                     RetroArch-Win64
 Intel                    'Ship of Harkinian'
 ```

 Pokud chcete ve svém domovském adresáři vytvořit odkaz na nově připojený disk, můžete spustit následující příkaz

 ```sh
 ❯ ln -s /media/windows ~/Windows
 ```

 Pro ověření, že to funguje

 ```sh
 ❯ ls ~/Windows
 '$Recycle.Bin'             Linux                  SteamLibrary
 AMD                       Modding                swapfile.sys
 Apps                      pagefile.sys          'System Volume Information'
 bootTel.dat               PerfLogs               Users
 Development               ProgramData            WiiU
'Documents and Settings'  'Program Files'         Windows
 DumpStack.log.tmp        'Program Files (x86)'   XboxGames
 FanControl                Recovery               xiv_modding
 Games                     RetroArch-Win64
 Intel                    'Ship of Harkinian'
 ```

## tl;dr

- Najděte UUID vašeho oddílu
```sh
lsblk -f
```

- Otevřete /etc/fstab
```sh
sudo nano /etc/fstab
```

- Vytvořte záznam na konci souboru
```sh
UUID=<UUID oddílu> /media/foo somefs defaults 0 0
```
Nahraďte `<UUID oddílu>`, `foo` a `somefs` vaším UUID, adresářem a souborovým systémem. Např. ext4, a také nastavte další možnosti, které můžete chtít za defaults, jako například `_netdev` pro NAS, nebo `nofail` pro jakýkoli nekritický disk.

- Znovu načtěte démona

```sh
❯ sudo systemctl daemon-reload
```

- Připojte váš disk
```sh
❯ sudo mount -a
```

Tento disk je nyní připojen a bude připojen i při budoucích spuštěních.

## Další četba
- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html - Standard hierarchie souborového systému
- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s11.html - FHS na `/media/`
- https://linux.die.net/man/8/dump - manuálová stránka pro `dump`
- https://man.archlinux.org/man/fsck.8 - manuálová stránka pro `fsck`
- https://man.archlinux.org/man/fstab.5.en - manuálová stránka pro fstab
- https://wiki.archlinux.org/title/Fstab - Arch Linux wiki pro fstab
