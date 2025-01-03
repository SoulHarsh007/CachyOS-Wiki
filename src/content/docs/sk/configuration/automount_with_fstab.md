---
title: Automatické pripájanie ďalších diskov cez fstab pri štarte
description: Pripojte ďalšie statické disky pri štarte pomocou súboru nachádzajúceho sa v /etc/fstab
---

Tento tutoriál popíše základy používania súboru fstab, ktorý sa nachádza v /etc/, na pripojenie statických diskov počas štartu. Stručne vysvetlí, ako nájsť UUID partície alebo disku, čo robia niektoré možnosti a ďalšie čítanie, ak poskytnuté informácie nebudú postačujúce.

## Predpoklady
- Prístup root

## Pridávanie záznamov do /etc/fstab

### 1. Zobrazenie UUID vašich partícií
V emulátore terminálu podľa vášho výberu (Konsole, Alacritty, Kitty, atď.) spustite nasledovné:

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

V našom príklade vieme, že chceme pripojiť partíciu Windows, ktorá je ntfs, a vieme, že približne polovica jej priestoru je voľná. Preto môžeme určiť, že partícia, ktorú chceme pripojiť, je `nvme0n1p3` a jej UUID je `08A24E90A24E81E4`, so systémom súborov `ntfs` v tomto príklade.

### 2. Identifikácia vašej partície

Často `lsblk -f` poskytne všetky informácie, ktoré potrebujete na pripojenie vášho disku cez /etc/fstab v tomto bode. Ak by ste však zistili, že informácie sú nedostatočné, môžete spustiť nasledovné:

```sh
❯ sudo fdisk -l
Device              Start        End    Sectors  Size Type
/dev/nvme0n1p1       2048     206847     204800  100M EFI System
/dev/nvme0n1p2     206848     239615      32768   16M Microsoft reserved
/dev/nvme0n1p3     239616 2997384182 2997144567  1.4T Microsoft basic data
/dev/nvme0n1p4 2997385216 2999482367    2097152    1G EFI System
/dev/nvme0n1p5 2999482368 3905454079  905971712  432G Linux root (x86-64)
/dev/nvme0n1p6 3905454080 3907026943    1572864  768M Windows recovery environment
```

V tomto príklade už poznáme naše UUID, avšak `fdisk -l` nám to môže trochu viac objasniť zobrazením presnej veľkosti partície (1.4T), ako aj jej typu (Microsoft basic data).

To by nám malo byť úplne jasné, že partícia, ktorú chceme, je `nvme0n1p3` s UUID `08A24E90A24E81E4`, ako bolo popísané skôr. Vedeli sme to už predtým, ale teraz to vieme s istotou.

Keď ste si istí, že ste našli správnu partíciu, skopírujte UUID. Kopírovanie z emulátora terminálu sa zvyčajne vykonáva pomocou `ctrl+shift+C`.

### 3. Pridávanie záznamu do /etc/fstab

Teraz, keď sme získali UUID našej partície, je čas otvoriť súbor fstab.

Môžete použiť textový editor podľa vlastného výberu, v tomto príklade použijeme nano. Na úpravu súboru fstab musí byť otvorený ako root:

```sh
❯ sudo nano /etc/fstab
```

Pomocou klávesov so šípkami prejdite na koniec súboru fstab a potom na novom riadku vytvoríme náš nový záznam:

```sh
UUID=08A24E90A24E81E4 /media/windows ntfs3 defaults,nofail 0 0
```
Rozpis tohto záznamu je nasledovný:

- `UUID=08A24E90A24E81E4` Toto je systém súborov, ktorý chceme pripojiť, identifikovaný pomocou jeho UUID. Existujú aj iné metódy na identifikáciu vášho systému súborov, hoci UUID býva najbezpečnejšie. Ďalšie metódy sú uvedené [tu](https://wiki.archlinux.org/title/Fstab#Identifying_file_systems).

- `/media/windows` [Linux Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html) hovorí, že `/media/` je správne umiestnenie pre pripojenie vymeniteľných diskov. `windows` označuje adresár, do ktorého chceme pripojiť náš disk. Každý disk, ktorý chceme pripojiť, bude potrebovať svoj vlastný adresár.

- `ntfs3` Toto je typ systému súborov pre náš systém súborov. V našom príklade explicitne používame ovládač jadra ntfs3. Ďalšími príkladmi by boli `ext4`, `xfs` alebo podobné. Toto explicitné vyhlásenie typu systému súborov je možné nahradiť slovom `auto`, aby príkaz mount mohol urobiť svoj najlepší odhad.

- `defaults,nofail` Možnosti, ktoré chceme odovzdať príkazu mount pre tento disk. `nofail` znamená, že ak sa tento disk nepodarí pripojiť, nespôsobí chybu počas štartu. Štart bude pokračovať normálne. `defaults` implikuje štandardnú sadu logických možností. Typicky `rw`, `ro` alebo podobné.

- `prvá 0` dump, toto je zvyčajne zastarané v moderných systémoch. Ponechanie na 0 ničomu neuškodí. Viac si o tom môžete prečítať [tu](https://linux.die.net/man/8/dump).

- `druhá 0` Toto nastavuje poradie kontroly systému súborov pri štarte. Pre koreňovú partíciu (pokiaľ váš koreňový systém súborov nie je btrfs alebo xfs, ktorý by mal byť nastavený na 0) by to malo byť 1. Všetky ostatné systémy súborov vo vašom fstab by mali byť buď 0 (vypnuté) alebo 2. Viac informácií [tu](https://man.archlinux.org/man/fsck.8).

Možnosti sú vysvetlené [tu](https://man7.org/linux/man-pages/man5/fstab.5.html) a [tu](https://man7.org/linux/man-pages/man8/mount.8.html) oveľa podrobnejšie.

#### Viac info
Mimochodom, všetky možnosti za deklaráciou typu systému súborov sú voliteľné, ak ich nezmeníte z predvolených.

Teda

`UUID=<UUID partície> /media/foo nejakýfs`

a

`UUID=<UUID partície> /media/foo nejakýfs defaults 0 0`

sú ekvivalentné. Za `nejakýfs` nasledované ničím je implicitne `nejakýfs defaults 0 0`

#### Dôležité pre Windows partície

Ak sa riadite touto príručkou s Windows partíciou, vaše možnosti by mali byť `uid=1000,gid=1000,rw,user,exec,umask=000`, pričom uid a gid nahradíte ID používateľa a ID skupiny. Ak neudelíte povolenia user a exec, Windows môže uzamknúť váš disk, takže nebudete môcť nič upravovať. Toto sa môže stať bez ohľadu na povolenia, ak nevypnete rýchle spustenie.

Ak nenastavíte umask=000, niektoré súbory môžu byť nezapisovateľné v závislosti od nastavení



### 4. Dokončenie

Ak chcete pripojiť disk, pre ktorý ste vytvorili záznam, spustite nasledovné:

```sh
❯ sudo systemctl daemon-reload
```

a potom:

```sh
❯ sudo mount -a
```

Váš disk by sa teraz mal zobraziť pod `/media/windows` a bude sa tam zobrazovať aj pri ďalšom štarte, ako aj v budúcnosti.

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

 Ak si želáte vytvoriť odkaz na váš novo pripojený disk vo vašom domovskom adresári, môžete spustiť nasledovné

 ```sh
 ❯ ln -s /media/windows ~/Windows`
 ```

 Pre zobrazenie, že to funguje

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

- Nájdite UUID vašej partície
```sh
lsblk -f
```

- Otvorte /etc/fstab
```sh
sudo nano /etc/fstab
```

- Vytvorte záznam na konci súboru
```sh
UUID=<UUID partície> /media/foo nejakýfs defaults 0 0
```
Nahraďte `<UUID partície>`, `foo` a `nejakýfs` vaším UUID, adresárom a systémom súborov. napr. ext4, ako aj nastavením akýchkoľvek ďalších možností, ktoré môžete chcieť po defaults, ako napríklad `_netdev` pre NAS alebo `nofail` pre akýkoľvek nekritický disk.

- Znova načítajte váš démon

```sh
❯ sudo systemctl daemon-reload
```

- Pripojte váš disk
```sh
❯ sudo mount -a
```

Tento disk je teraz pripojený a odteraz sa bude pripájať aj pri štarte.

## Ďalšie čítanie
- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html - Filesystem Hierarchy Standard
- https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch03s11.html - FHS o `/media/`
- https://linux.die.net/man/8/dump - manuál pre `dump`
- https://man.archlinux.org/man/fsck.8 - manuál pre `fsck`
- https://man.archlinux.org/man/fstab.5.en - manuálová stránka pre fstab
- https://wiki.archlinux.org/title/Fstab - Arch Linux wiki pre fstab
