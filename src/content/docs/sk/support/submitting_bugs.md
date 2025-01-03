---
title: Podávanie chýb
---

# Opíšte svoj problém

- *Čo nefunguje?*
- *Rieši problém zníženie verzie balíčka X?*
- *Použili ste vyhľadávaciu funkciu na rovnaké problémy?*
- *Urobili ste nejaké úpravy sami?*
  - Príklad: `Pridanie ďalšieho parametra v súbore modprobe`

# Poskytnite logy

CachyOS ponúka skvelý nástroj na zber logov zo systému s názvom `cachyos-bugreport.sh`. Tento nástroj zhromaždí logy z:
- dmesg
- journalctl
- inxi `(na zhromaždenie informácií o hardvéri)`

Keď sú logy zhromaždené, používateľ bude vyzvaný, aby sa rozhodol, či ich chce nahrať na našu stránku pre zdieľanie.

**Spustite nasledujúci príkaz v termináli a priložte odkaz s chybou do témy:**
```sh
sudo cachyos-bugreport.sh
```

# Odkazy na podanie hlásenia

- GitHub: <https://github.com/CachyOS/distribution>
- Fórum: <https://discuss.cachyos.org/c/feedback/bugreports/10>
- Discord: [Kanál podpory](https://discord.com/channels/862292009423470592/862294383470051348)
