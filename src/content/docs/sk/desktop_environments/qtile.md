---
title: Konfigurácia Qtile
description: Klávesové skratky a FAQ pre CachyOS Qtile
---

Poďakovanie patrí [Shendisxovi](<https://github.com/Shendisx>) za vytvorenie tejto konfigurácie Qtile.

> X11 a Wayland sedenie

## Klávesové skratky

Väčšina kombinácií klávesov vyžaduje použitie modifikačného klávesu, ktorým je v našom prípade kláves Windows (označovaný ako SUPER), môžete ho zmeniť v konfiguračnom súbore.
Niektoré z nich môžu používať mod1 (kláves ALT).

### Otvoriť terminál

* SUPER + Return

### Ukončiť zamerané okno

* SUPER + Q

### Prejsť na pracovnú plochu (1-9)

* SUPER + 1-9 (číselný riadok, numerická klávesnica sa nepočíta)

### Otvoriť Rofi (spúšťač programov)

* ALT + Medzerník

### Presunúť zameranie na (Vľavo, Vpravo, Dole, Hore)

* SUPER + H (Vľavo)
* SUPER + L (Vpravo)
* SUPER + J (Dole)
* SUPER + K (Hore)
* SUPER + Medzerník (Presúvať okná medzi ľavým/pravým stĺpcom alebo presúvať hore/dole v aktuálnom zásobníku)

### Presunúť zamerané okno na (Vľavo, Vpravo, Dole, Hore)

* SUPER + Shift + H (Vľavo)
* SUPER + Shift + L (Vpravo)
* SUPER + Shift + J (Dole)
* SUPER + Shift + K (Hore)

### Zväčšiť zamerané okno na (Vľavo, Vpravo, Dole, Hore)

* SUPER + Control + H (Vľavo)
* SUPER + Control + L (Vpravo)
* SUPER + Control + J (Dole)
* SUPER + Control + K (Hore)

### Obnoviť všetky veľkosti okien aktuálnej pracovnej plochy na ich pôvodnú veľkosť

* SUPER + N

### Prepnuť režim celej obrazovky v zameranom okne

* SUPER + F

### Prepnuť plávajúce okno v zameranom okne

* SUPER + V

### Prepínať medzi rozdelenými a nerozdelenými stranami zásobníka

* SUPER + Shift + Return

### Prepínať medzi rozloženiami

* SUPER + TAB

### Znovu načítať konfiguračný súbor Qtile

* SUPER + Control + R

### Ukončiť Qtile (ukončiť bežiace X sedenie)

* SUPER + Control + Q

### Spustiť Flameshot (nástroj na vytváranie snímok obrazovky)

* Print

### Zachytiť snímku obrazovky celej obrazovky (Uložené v $HOME/Pictures)

* Control + Print

### Otvoriť správcu súborov (predvolene Thunar)

* SUPER + E

### Presúvať plávajúce okno pomocou myši

* SUPER + Ľavé tlačidlo myši

### Zväčšovať plávajúce okno pomocou myši

* SUPER + Pravé tlačidlo myši

### Presunúť okno dopredu

* SUPER + Tlačidlo kolieska myši

### Prilepiť okno (Napríklad prilepenie Firefox PIP vás teraz bude nasledovať medzi pracovnými plochami)

* SUPER + S

## FAQ

### Prečo widget hlasitosti zobrazuje chybu alebo je zaseknutý na 0%?

* Niekedy je to spôsobené tým, že widget hlasitosti Qtile nedokáže rozpoznať vaše predvolené výstupné zariadenie. Viac informácií nájdete vo wiki.
* <https://docs.qtile.org/en/latest/manual/ref/widgets.html#pulsevolume>

### Existuje skript autostart.sh?

* Nachádza sa v scripts/ v priečinku Qtile.

### Interaguje lišta Qtile s myšou?

* Áno, napríklad ak posúvate kolieskom na malých bodkách, ktoré predstavujú vaše pracovné plochy (Aktívne, Neaktívne, Prázdne atď.), prepnete sa na ľavú alebo pravú, alebo na ne dokonca kliknete.
* Ďalším príkladom je rozloženie (predvolene stĺpce), kliknutím naň môžete prepínať medzi dostupnými rozloženiami.
* Využitie CPU a RAM sa kliknutím otvorí Btop (TUI System Monitor).
* Zvýšenie/Zníženie/Stlmenie/ interakciou s widgetom hlasitosti.

Pre viac informácií o Qtile si prosím pozrite ich wiki pre referencie.

* <https://docs.qtile.org/en/stable/>
