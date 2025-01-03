---
title: Konfigurácia Hyprland
description: Klávesové skratky CachyOS Hyprland & FAQ
---

:::caution
Keďže Hyprland začal s prepracovaním, prosím, berte na vedomie, že aktuálne nie je stabilný a môžete sa stretnúť s chybami/neočakávanými pádmi. Používate ho na vlastné riziko.
Dokonca aj ich "stabilná" verzia je tiež pokazená a chybná, preto neplánujeme poskytovať podporu mimo našich dotfiles. Namiesto toho sa obráťte na ich [wiki](<https://wiki.hyprland.org/>).
:::

:::tip
Spustite Hyprland pomocou non-systemd položky, inak sa nespustí a skončíte s čiernou obrazovkou.

Príklad: **`Hyprland`** namiesto **`Hyprland(systemd)`**.
:::

Naším hlavným cieľom s naším nastavením je mať funkčný Hyprland, ale zachovať ho jednoduchý, preto môžu chýbať niektoré základné nástroje a programy, ako napríklad GUI správca súborov.

Pozrite si naše [Hyprland FAQ.](/desktop_environments/hyprland#faq)

**Dotfiles spravované [msmafra](https://github.com/msmafra) a [Lysec](https://github.com/Ly-sec)**

## Klávesové skratky

Väčšina kombinácií kláves vyžaduje použitie modifikačného klávesu, ktorým je v našom prípade kláves Windows (označovaná ako SUPER), môžete ho zmeniť v konfiguračnom súbore.

### Otvoriť terminál

* SUPER + Return

### Prepnúť na pracovnú plochu (1-9)

* SUPER + 1-9 (Číselný riadok, numerická klávesnica sa nepočíta)

### Presunúť zameranie na (Vľavo,Vpravo,Hore,Dole)

* SUPER + Šípky

### Presúvať sa medzi pracovnými plochami pomocou kolieska myši

* Super + Scroll

### Presúvať sa medzi pracovnými plochami pomocou čiarky a bodky

* Super + bodka (Ďalšia pracovná plocha)
* Super + čiarka (Predchádzajúca pracovná plocha)

### Presunúť zamerané okno na pracovnú plochu (1-9), ale nepreniesť sa tam

* SUPER + Shift + 1-9

### Rovnaké ako vyššie, ale aj prepnúť na danú pracovnú plochu

* SUPER + CTRL + 1-9

### Otvoriť Rofi (Spúšťač programov)

* CTRL + Medzerník

### Zavrieť zamerané okno

* SUPER + Q

### Presunúť zamerané okno v smere (Hore,Dole,Vľavo,Vpravo)

* SUPER + Shift + Šípky

### Zmeniť veľkosť zameraného okna

* SUPER + CTRL + Shift + J (Smerom nadol)
* SUPER + CTRL + Shift + K (Smerom nahor)
* SUPER + CTRL + Shift + H (Vľavo)
* SUPER + CTRL + Shift + L (Vpravo)
* SUPER + CTRL + Shift + Šípka (Akýkoľvek smer)

### Prepnúť zamerané okno do stavu Floating alebo Fullscreen

* SUPER + F (Fullscreen)
* SUPER + V (Floating)

### Vstúpiť do stavu podmapy zmeny veľkosti (Umožňuje zmenu veľkosti), H,J,K,L alebo pomocou šípok

* SUPER + R
* ESC na ukončenie

### Presúvať okno ťahaním myšou

* SUPER + Ľavé kliknutie

### Zmeniť veľkosť okna

* SUPER + Pravé kliknutie (držte ho stlačené a ťahajte kurzor v ľubovoľnom smere)

### Ovládanie hlasitosti (Multimediálne klávesy) ako napríklad Zvýšenie hlasitosti, Zníženie hlasitosti a Stlmenie

### Ovládanie jasu by malo fungovať v závislosti od hardvéru

### Ovládanie prehrávania na pozastavenie, prehrávanie, nasledujúce a predchádzajúce pomocou multimediálnych kláves (Laptop alebo klávesnica)

### Pripnúť zamerané okno, aby sa zobrazovalo na všetkých pracovných plochách (Floating)

* SUPER + Y

### Prepnúť aktuálne okno do skupiny

* SUPER + K

### Zmeniť aktívnu skupinu

* SUPER + TAB

### Znovu načítať Waybar

* SUPER + O

### Znížiť medzeru medzi oknami

* SUPER + G

### Obnoviť medzery na predvolenú hodnotu

* SUPER + Shift + G

### Otvoriť správcu súborov (Premenná nie je predvolene nakonfigurovaná)

* SUPER + E

### Snímka obrazovky

* Print (PrtSc)

## FAQ

### Prečo má môj Discord, Thunar a Nautilus zvláštne pozadie?

Je to preto, že okno má upravenú priehľadnosť.

* Zvážte úpravu pravidla okna v konfiguračnom súbore [Hyprland](https://github.com/CachyOS/cachyos-hyprland-settings/blob/master/etc/skel/.config/hypr/config/windowrules.conf#L21).

```sh title='Príklad'
windowrulev2 = opacity 0.92, class:^(thunar|nemo)$
```

### Je zahrnutý správca súborov?

* Nie, nainštalujte si takého, ktorý sa vám páči.
