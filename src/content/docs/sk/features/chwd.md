---
title: Detekcia Hardvéru CachyOS
description: Detekcia a konfigurácia hardvéru pre CachyOS
---

[Detekcia hardvéru CachyOS](https://github.com/CachyOS/chwd/) alebo známejšia ako **`chwd`** nám umožňuje podporovať rôzne druhy hardvéru inštaláciou potrebných
balíčkov a ovládačov pre spustený systém. To zahŕňa systémy s grafickými kartami NVIDIA, T2 Macbooky a handheld zariadenia ako Steam Deck a ROG Ally.

## Použitie

**`chwd`** sa zvyčajne spúšťa počas inštalácie na poskytnutie potrebných balíčkov pre systém. Je však tiež možné
ho použiť po inštalácii.

### Automatická Konfigurácia

**`chwd`** podporuje inštaláciu a konfiguráciu potrebných ovládačov a balíčkov, aby systém mohol pracovať v optimálnych podmienkach.

```sh
❯ sudo chwd -a
```

### Inštalácia profilu

Alternatívou k vyššie uvedenému spôsobu je inštalácia každého špecifického profilu.

```sh title='Zoznam všetkých dostupných profilov'
❯ chwd --list-all
╭─────────────────────────┬─────────╮
│ Názov                    ┆ NonFree │
╞═════════════════════════╪═════════╡
│ nvidia-open-dkms.prime  ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ nvidia-dkms             ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ macbook-t2              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ phoenix                 ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ steam-deck              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ amd                     ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ intel                   ┆ false   │
╰─────────────────────────┴─────────╯
```

```sh title='Inštalácia chwd profilu'
❯ sudo chwd -i amd
> Inštaluje sa amd ...

> Úspešne nainštalované amd
```

### Ostatné

Syntax príkazov a ďalšie použitie nájdete vo výstupe pomocníka pre **`chwd`**.

```sh
❯ chwd --help
Použitie: chwd [OPTIONS]

Možnosti:
  -i, --install <profile>          Inštalovať profil
  -r, --remove <profile>           Odstrániť profil
  -d, --detail                     Zobraziť podrobné informácie pre výpisy
  -f, --force                      Vynútiť preinštaláciu
      --list-installed             Zoznam nainštalovaných jadier
      --list                       Zoznam dostupných profilov pre všetky zariadenia
      --list-all                   Zoznam všetkých profilov
  -a, --autoconfigure [<classid>]  Automatická konfigurácia
      --ai_sdk                     Prepnúť profily AI SDK
      --pmcachedir <PMCACHEDIR>    [predvolené: /var/cache/pacman/pkg]
      --pmconfig <PMCONFIG>        [predvolené: /etc/pacman.conf]
      --pmroot <PMROOT>            [predvolené: /]
  -h, --help                       Zobraziť pomocníka
  -V, --version                    Zobraziť verziu
```
