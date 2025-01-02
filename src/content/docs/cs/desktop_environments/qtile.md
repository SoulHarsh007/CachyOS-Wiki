---
title: Konfigurace Qtile
description: Klávesové zkratky a FAQ pro CachyOS Qtile
---

Zásluhy za vytvoření tohoto nastavení Qtile jdou [Shendisxovi](<https://github.com/Shendisx>).

> Sezení X11 a Wayland

## Klávesové zkratky

Většina kombinací kláves vyžaduje použití modifikační klávesy, která je v našem případě klávesa Windows (označovaná jako SUPER), můžete ji změnit v konfiguračním souboru.
Některé z nich mohou používat mod1 (klávesa ALT).

### Otevření terminálu

* SUPER + Return

### Zavření okna v popředí

* SUPER + Q

### Přechod na pracovní plochu (1-9)

* SUPER + 1-9 (číselná řada, numerická klávesnice se nepočítá)

### Otevření Rofi (spouštěč programů)

* ALT + Mezerník

### Přesun zaměření na (vlevo, vpravo, dolů, nahoru)

* SUPER + H (vlevo)
* SUPER + L (vpravo)
* SUPER + J (dolů)
* SUPER + K (nahoru)
* SUPER + Mezerník (Přesun oken mezi levým/pravým sloupcem nebo přesun nahoru/dolů v aktuálním zásobníku)

### Přesun okna v popředí na (vlevo, vpravo, dolů, nahoru)

* SUPER + Shift + H (vlevo)
* SUPER + Shift + L (vpravo)
* SUPER + Shift + J (dolů)
* SUPER + Shift + K (nahoru)

### Zvětšení okna v popředí na (vlevo, vpravo, dolů, nahoru)

* SUPER + Control + H (vlevo)
* SUPER + Control + L (vpravo)
* SUPER + Control + J (dolů)
* SUPER + Control + K (nahoru)

### Obnovení všech velikostí oken aktuální pracovní plochy na jejich původní velikost

* SUPER + N

### Přepnutí okna v popředí do režimu celé obrazovky

* SUPER + F

### Přepnutí plovoucího okna v popředí

* SUPER + V

### Přepínání mezi rozdělenými a nerozdělenými stranami zásobníku

* SUPER + Shift + Return

### Přepínání mezi rozloženími

* SUPER + TAB

### Znovunačtení konfiguračního souboru Qtile

* SUPER + Control + R

### Ukončení Qtile (ukončení běžícího X sezení)

* SUPER + Control + Q

### Spuštění Flameshot (nástroj pro snímání obrazovky)

* Print

### Zachycení snímku celé obrazovky (uloženo v $HOME/Pictures)

* Control + Print

### Otevření správce souborů (ve výchozím nastavení Thunar)

* SUPER + E

### Přetažení plovoucího okna myší

* SUPER + Levé tlačítko myši

### Zvětšení plovoucího okna myší

* SUPER + Pravé tlačítko myši

### Přenesení okna do popředí

* SUPER + Tlačítko kolečka myši

### Přilepení okna (například přilepení Firefox PIP vás bude nyní sledovat mezi pracovními plochami)

* SUPER + S

## FAQ

### Proč widget hlasitosti zobrazuje chybu nebo je zaseknutý na 0 %?

* Někdy je to kvůli tomu, že widget hlasitosti Qtile nedokáže detekovat vaše výchozí výstupní zařízení, více informací naleznete ve wiki.
* <https://docs.qtile.org/en/latest/manual/ref/widgets.html#pulsevolume>

### Existuje skript autostart.sh?

* Nachází se ve složce scripts/ ve složce Qtile

### Interaguje lišta Qtile s myší?

* Ano, například pokud budete posouvat kolečkem myši na malých tečkách, které představují vaše pracovní plochy (aktivní, neaktivní, prázdné atd.), přepnete se na levou nebo pravou, nebo na jednu z nich kliknete.
* Dalším příkladem je rozložení (ve výchozím nastavení sloupce), kliknutím na něj můžete přepínat mezi dostupnými rozloženími
* Využití CPU a RAM kliknutím na něj otevřete Btop (TUI System Monitor)
* Zvýšení/snížení/ztlumení/interakcí na widgetu hlasitosti

Další informace o Qtile naleznete v jejich wiki.

* <https://docs.qtile.org/en/stable/>
