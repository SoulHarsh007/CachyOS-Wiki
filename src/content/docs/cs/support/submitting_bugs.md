---
title: Podávání hlášení o chybách
---

# Popište váš problém

- *Co nefunguje?*
- *Opraví problém downgradování balíčku X?*
- *Použili jste funkci hledání pro podobné problémy?*
- *Provedli jste nějaké vlastní úpravy?*
  - Příklad: `Přidání dalšího příznaku v souboru modprobe`

# Poskytněte logy

CachyOS nabízí skvělý nástroj pro shromažďování logů ze systému nazvaný `cachyos-bugreport.sh`.
Tento nástroj shromažďuje logy z:
- dmesg
- journalctl
- inxi `(pro získání informací o hardwaru)`

Když jsou logy shromážděny, uživatel bude vyzván k rozhodnutí, zda je chce nahrát na náš paste web.

**Spusťte následující příkaz v terminálu a přidejte odkaz spolu s hlášením o chybě do tématu:**
```sh
sudo cachyos-bugreport.sh
```

# Odkazy pro podání hlášení

- GitHub: <https://github.com/CachyOS/distribution>
- Fórum: <https://discuss.cachyos.org/c/feedback/bugreports/10>
- Discord: [Kanál podpory](https://discord.com/channels/862292009423470592/862294383470051348)
