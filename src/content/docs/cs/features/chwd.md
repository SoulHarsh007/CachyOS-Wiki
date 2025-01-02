---
title: Detekce hardwaru v CachyOS
description: Detekce a konfigurace hardwaru pro CachyOS
---

[CachyOS Hardware Detection](https://github.com/CachyOS/chwd/) neboli zkráceně **`chwd`** umožňuje zprovoznit širokou škálu hardwaru instalací nezbytných
balíčků a ovladačů pro daný systém. To zahrnuje systémy s grafickými kartami NVIDIA, počítače T2 Macbook a handheldy jako Steam Deck a ROG Ally.

## Použití

**`chwd`** se obvykle spouští během instalace, aby poskytl potřebné balíčky pro systém. Je ale také možné
použít ho i po instalaci.

### Automatická konfigurace

**`chwd`** podporuje instalaci a konfiguraci nezbytných ovladačů a balíčků, aby systém mohl pracovat za optimálních podmínek.

```sh
❯ sudo chwd -a
```

### Instalace profilu

Alternativou k výše uvedené metodě je instalace specifického profilu.

```sh title='Zobrazení všech dostupných profilů'
❯ chwd --list-all
╭─────────────────────────┬─────────╮
│ Název                   ┆ NonFree │
╞═════════════════════════╪═════════╡
│ nvidia-open-dkms.prime   ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ nvidia-dkms             ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ macbook-t2              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ phoenix                  ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ steam-deck              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ amd                     ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ intel                   ┆ false   │
╰─────────────────────────┴─────────╯
```

```sh title='Instalace profilu chwd'
❯ sudo chwd -i amd
> Instaluji amd ...

> Úspěšně nainstalováno amd
```

### Ostatní

Pro syntaxi příkazů a další možnosti použití nahlédněte do nápovědy **`chwd`**.

```sh
❯ chwd --help
Použití: chwd [MOŽNOSTI]

Možnosti:
  -i, --install <profil>          Nainstaluje profil
  -r, --remove <profil>           Odstraní profil
  -d, --detail                    Zobrazí detailní informace pro výpisy
  -f, --force                     Vynutí přeinstalaci
      --list-installed            Zobrazí nainstalovaná jádra
      --list                      Zobrazí dostupné profily pro všechna zařízení
      --list-all                  Zobrazí všechny profily
  -a, --autoconfigure [<classid>] Automatická konfigurace
      --ai_sdk                    Přepne profily AI SDK
      --pmcachedir <PMCACHEDIR>   [výchozí: /var/cache/pacman/pkg]
      --pmconfig <PMCONFIG>        [výchozí: /etc/pacman.conf]
      --pmroot <PMROOT>            [výchozí: /]
  -h, --help                      Zobrazí nápovědu
  -V, --version                   Zobrazí verzi
```
