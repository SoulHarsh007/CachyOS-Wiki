---
title: Ponúkaní správcovia zavádzania
description: Popis a odporúčania pre aktuálne ponúkaných správcov zavádzania
---

Aby CachyOS ponúkal najlepší zážitok na rôznych zariadeniach, aktuálne ponúka nasledujúcich správcov zavádzania: systemd-boot, rEFInd a GRUB.
Tento článok na Wiki popisuje sadu funkcií každého správcu zavádzania a obsahuje aj naše odporúčania pri ich výbere. Pre
konfiguráciu si pozrite [Konfigurácia správcu zavádzania](/configuration/boot_manager_configuration).

## systemd-boot
Ako súčasť rodiny systemd bol systemd-boot vytvorený tak, aby bol čo najjednoduchší, preto má podporu iba pre systémy založené na UEFI. Tento jednoduchý, ale efektívny dizajn zaisťuje jeho spoľahlivosť a rýchlosť. Avšak toto má za následok stratu pokročilých funkcií podporovaných inými správcami zavádzania.

### Výhody
- Veľmi jednoduchá konfigurácia.
- Záznamy o zavádzaní sú oddelené do viacerých súborov, čo uľahčuje správu.

### Nevýhody
 - Nepodporuje systémy BIOS.
 - Veľmi jednoduchý dizajn a chýba akýkoľvek druh tém alebo prispôsobenia.
 - Konfigurácia sa negeneruje automaticky, pokiaľ nie je nakonfigurovaná. CachyOS obsahuje systemd-boot manager na ponuku automaticky generovanej konfigurácie.
 - Dokáže čítať iba zavádzacie obrazy na súborových systémoch podporovaných EFI (FAT, FAT16, FAT32).
 - Neschopnosť nájsť zavádzacie obrazy na iných partíciách ako na svojej vlastnej.

### Odporúčanie
Systemd-boot je odporúčaný a predvolený správca zavádzania pre CachyOS. Vyberte si ho, ak si nie ste istí.

## rEFInd
Ako fork rEFIt bol rEFInd primárne vytvorený, aby používateľom MacOS uľahčil viacnásobné zavádzanie. Avšak rEFInd sa vyvinul tak, že je agnostický voči hardvéru, čo z neho robí skvelú voľbu pre viacnásobné zavádzanie na akomkoľvek systéme. Hlavným lákadlom rEFInd je jeho schopnosť skenovať všetky úložné zariadenia pri zavádzaní a zodpovedajúcim spôsobom zobrazovať záznamy pre každý nájdený OS/jadro.

### Výhody
- Automatická detekcia všetkých operačných systémov a jadier na úložných zariadeniach.
- Kvôli spomínanej automatickej detekcii sa vyžaduje malá alebo žiadna konfigurácia.
- Oveľa grafickejšie používateľské rozhranie pripomínajúce výber zavádzania v MacOS.
- Skvelá podpora tém.
- Dokáže čítať zavádzacie obrazy zo súborových systémov EFI (FAT, FAT16, FAT32), ako aj EXT4 a BTRFS.

### Nevýhody
- Nepodporuje systémy BIOS.

### Odporúčanie
rEFInd je odporúčaný správca zavádzania pre zavádzanie s viacerými operačnými systémami.

## GRUB
GRUB je najstarší z dostupných správcov zavádzania a v dôsledku toho jediný, ktorý podporuje zavádzanie cez BIOS. Má veľmi rozsiahlu sadu funkcií, funguje takmer na každom počítači a je najčastejšie používaným správcom zavádzania Linuxu.
Nasleduje zoznam jeho hlavných výhod a nevýhod.

### Výhody
- Dokáže čítať zavádzacie obrazy takmer zo všetkých dostupných súborových systémov Linuxu.
- Široko používaný a veľmi ľahko sa dajú nájsť informácie online.
- Dokáže dešifrovať šifrované zavádzacie partície.
- Jediný ponúkaný zavádzač, ktorý umožňuje zavádzanie počítačov s BIOSom.
- Vyzerá zastarane. Avšak má skvelú podporu tém na kompenzáciu.

### Nevýhody
- Nadmerný kvôli potrebe podpory oveľa staršieho hardvéru a potrebe množstva ovládačov súborových systémov.
- Pozorovateľne pomalší v porovnaní so systemd-boot a rEFInd.

### Odporúčanie
GRUB je jediný dostupný zavádzač, ktorý podporuje zavádzanie cez BIOS. Je to tiež jediný správca zavádzania, ktorý podporuje šifrovanie zavádzacej partície (odlišné od šifrovania disku).

## TL:DR
Vyberte GRUB, ak je používaný počítač iba s BIOSom, vyberte rEFInd, ak plánujete mať na počítači viacero operačných systémov (najmä Windows), inak použite systemd-boot.
