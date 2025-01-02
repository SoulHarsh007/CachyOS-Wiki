---
title: Konfigurace Hyprlandu
description: Klávesové zkratky a FAQ pro CachyOS Hyprland
---

:::caution
Vzhledem k tomu, že Hyprland zahájil přepracování, mějte na paměti, že v současnosti není stabilní a můžete se setkat s chybami/nečekanými pády. Používejte na vlastní nebezpečí.
Dokonce i jejich "stabilní" verze je také rozbitá a zabugovaná, proto neplánujeme poskytovat podporu mimo naše dotfiles. Místo toho se podívejte na jejich [wiki](<https://wiki.hyprland.org/>).
:::

:::tip
Spusťte Hyprland pomocí položky bez systemd, jinak se nespustí a povede k černé obrazovce.

Příklad: **`Hyprland`** místo **`Hyprland(systemd)`**.
:::

Naším hlavním cílem s naším nastavením je mít funkční Hyprland, ale zachovat ho jednoduchý, proto mohou chybět některé základní nástroje a programy, jako je GUI správce souborů.

Podívejte se do našeho [Hyprland FAQ.](/desktop_environments/hyprland#faq)

**Dotfiles udržují [msmafra](https://github.com/msmafra) a [Lysec](https://github.com/Ly-sec)**

## Klávesové zkratky

Většina kombinací kláves vyžaduje použití modifikační klávesy, která je v našem případě klávesa Windows (označovaná jako SUPER), můžete ji změnit v konfiguračním souboru.

### Otevření terminálu

* SUPER + Return

### Přechod na pracovní plochu (1-9)

* SUPER + 1-9 (číselná řada, numerická klávesnice se nepočítá)

### Změna zaměření na (vlevo, vpravo, nahoru, dolů)

* SUPER + Šipky

### Přesun mezi pracovními plochami pomocí kolečka myši

* Super + Kolečko myši

### Přesun mezi pracovními plochami pomocí čárky a tečky

* Super + . (Další pracovní plocha)
* Super + , (Předchozí pracovní plocha)

### Přesun okna v popředí na pracovní plochu (1-9), ale nepřecházejte tam

* SUPER + Shift + 1-9

### Stejné jako výše, ale také přepne na danou pracovní plochu

* SUPER + CTRL + 1-9

### Otevření Rofi (spouštěč programů)

* CTRL + Mezerník

### Zavření okna v popředí

* SUPER + Q

### Přesun okna v popředí na směr (nahoru, dolů, vlevo, vpravo)

* SUPER + Shift + Šipky

### Změna velikosti okna v popředí

* SUPER + CTRL + Shift + J (dolů)
* SUPER + CTRL + Shift + K (nahoru)
* SUPER + CTRL + Shift + H (vlevo)
* SUPER + CTRL + Shift + L (vpravo)
* SUPER + CTRL + Shift + Šipka (jakýkoli směr)

### Přepnutí okna v popředí do plovoucího nebo celoobrazovkového stavu

* SUPER + F (celá obrazovka)
* SUPER + V (plovoucí)

### Vstup do stavu podmapy změny velikosti (umožňuje změnu velikosti), H, J, K, L nebo pomocí šipek

* SUPER + R
* ESC pro ukončení

### Přesun okna tažením myši

* SUPER + Levé tlačítko myši

### Změna velikosti okna

* SUPER + Pravé tlačítko myši (podržte ho stisknuté a táhněte kurzorem v libovolném směru)

### Ovládání hlasitosti (multimediální klávesy), jako je VolUP, VolDOWN a MUTE

### Ovládání jasu by mělo fungovat v závislosti na hardwaru

### Ovládání přehrávání pro pozastavení, přehrávání, další a předchozí pomocí multimediálních kláves (notebook nebo klávesnice)

### Připnutí okna v popředí, aby se zobrazovalo na všech pracovních plochách (plovoucí)

* SUPER + Y

### Přepnutí aktuálního okna do skupiny

* SUPER + K

### Změna aktivní skupiny

* SUPER + TAB

### Znovunačtení Waybaru

* SUPER + O

### Zmenšení mezery mezi okny

* SUPER + G

### Obnovení mezer na výchozí hodnotu

* SUPER + Shift + G

### Otevření správce souborů (proměnná není ve výchozím nastavení nakonfigurována)

* SUPER + E

### Snímek obrazovky

* Print (PrtSc)

## FAQ

### Proč má můj Discord, Thunar a Nautilus divné pozadí?

To je proto, že okno má upravenou průhlednost

* Zvažte úpravu pravidla okna v konfiguračním souboru [Hyprland](https://github.com/CachyOS/cachyos-hyprland-settings/blob/master/etc/skel/.config/hypr/config/windowrules.conf#L21).

```sh title='Příklad'
windowrulev2 = opacity 0.92, class:^(thunar|nemo)$
```

### Je zahrnut správce souborů?

* Ne, nainstalujte si ten, který se vám líbí
