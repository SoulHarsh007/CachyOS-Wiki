---
title: Jádro CachyOS
description: Vlastnosti a změny v jádře CachyOS
---

Jádro CachyOS je upravené jádro, které využívá vylepšení, konfigurace a záplaty z upstreamu.

## Vlastnosti

- Vyberte si mezi 3 schedulery jádra a různými [sched-ext](/configuration/sched-ext) schedulery pro lepší odezvu
- Vylepšení AMD P-State
- Nejnovější BBRv3 od Googlu
- le9uo pro výrazně lepší odezvu při vysokém zatížení paměti
- Aktuální sada záplat NTSYNC, používaná s kompatibilním sestavením wine/protonu
- Kompatibilita se zařízeními T2 MacOS se záplatami z [t2linux](https://github.com/t2linux/linux-t2-patches/)
- Umožňuje čtení spotřeby energie CPU pro jednotlivá jádra pro uživatele AMD
- ACS Override a v412loopback
- Modul VHBA pro emulaci zařízení CD/DVD-ROM
- Nejnovější sada záplat ZSTD
- Různé další záplaty, které se zaměřují na zlepšení výkonu (optimalizované příznaky kompilátoru, vylepšení kryptografie, úpravy správy paměti)

Podrobnější seznam záplat, které CachyOS nabízí, naleznete v úplnějším
[seznamu funkcí](https://github.com/CachyOS/linux-cachyos/?tab=readme-ov-file#features), [repozitáři záplat jádra](https://github.com/CachyOS/kernel-patches)
a [zdrojovém stromu Linuxu CachyOS](https://github.com/CachyOS/linux).

## Varianty

CachyOS nabízí rozmanitou škálu možností jádra. Všechna jádra, která poskytujeme, jsou dodávána se [základní sadou záplat CachyOS](https://github.com/CachyOS/kernel-patches).
Pro každé z jader existuje [odpovídající varianta `-lto`](#konvence-pojmenování-balíčků), která je
sestavena pomocí [clang](https://clang.llvm.org/) namísto [GCC](https://gcc.gnu.org/). Výjimkou jsou výchozí jádro a jádro `-rc`, protože jsou
ve výchozím nastavení sestavena s [ThinLTO](https://blog.llvm.org/2016/06/thinlto-scalable-and-incremental-lto.html), a proto mají místo toho odpovídající varianty jádra `-gcc`.

- **linux-cachyos**
    - Výchozí jádro. Toto je doporučené jádro, pokud si nejste jisti, které jádro by se mělo použít.
    - Používá scheduler [BORE](https://github.com/firelzrd/bore-scheduler).
    - Ve výchozím nastavení je sestaveno s clang a ThinLTO pro produkci optimalizovanějších binárních souborů.
    - Profilováno s naším vlastním [AutoFDO](https://cachyos.org/blog/2411-kernel-autofdo/) profilem pro lepší výkon. [Skript](https://github.com/CachyOS/cachyos-benchmarker/blob/master/kernel-autofdo.sh) použitý k profilování jádra.
- **linux-cachyos-bore**
    - Používá scheduler BORE.
- **linux-cachyos-bmq**
    - Používá scheduler BMQ z [Project C](https://gitlab.com/alfredchen/projectc/) od Alfreda Chena.
        - **Nepodporuje sched-ext**.
- **linux-cachyos-deckify**
    - Výchozí jádro pro handheldy. **Nedoporučuje se** a **není podporováno** používat na handheldech jiné jádro než toto.
    - Používá scheduler BORE.
    - Specifické záplaty pro handheldy nad rámec základní sady záplat pro zlepšení kompatibility a celkového zážitku na handheldech.
- **linux-cachyos-eevdf**
    - Vylepšuje výchozí scheduler jádra pro lepší odezvu.
- **linux-cachyos-lts**
    - Založeno na nejnovějším jádře s dlouhodobou podporou.
    - Používá scheduler BORE.
    - Minimálně záplatováno ve srovnání s jinými jádry, aby byla zajištěna maximální stabilita.
- **linux-cachyos-hardened**
    - Používá scheduler BORE.
    - Zahrnuje sadu záplat [linux-hardened](https://github.com/anthraxx/linux-hardened).
    - Konfigurace jádra založená na [konfiguraci linux-hardened](https://gitlab.archlinux.org/archlinux/packaging/packages/linux-hardened/-/blob/main/config).
        - Obsahuje velmi agresivní hardening, který výrazně snižuje výkon a uživatelský komfort.
        - **Nepodporuje sched-ext**.
- **linux-cachyos-rc**
    - Založeno na nejnovějším jádře mainline z [Linusova stromu](https://github.com/torvalds/linux/).
    - Používá scheduler BORE.
    - Hlavní jádro pro zavádění nových funkcí do naší sady záplat.
- **linux-cachyos-server**
    - Laděno pro serverové úlohy ve srovnání s desktopovým použitím.
        - Frekvence časovače 300 Hz.
        - Bez preempce.
        - Základní EEVDF.
- **linux-cachyos-rt-bore**
    - Preempce v reálném čase.
    - Používá scheduler BORE.

Otevřete prosím issue v [linux-cachyos GitHub](https://github.com/CachyOS/linux-cachyos) pro návrhy a vylepšení, která lze přidat do výchozího jádra.

## Předkompilované moduly jádra

Aby se přizpůsobilo širší uživatelské základně, CachyOS dodává s jádrem některé známé a hojně používané moduly jádra. To znamená, že uživatelé již nebudou muset
po každé aktualizaci jádra nebo při každé instalaci nového jádra tyto moduly znovu kompilovat, ale budou je muset pouze nainstalovat z repozitáře, protože jsou
již předkompilované. To efektivně zneplatňuje všechny balíčky `-dkms`, které uživatel může mít a které poskytují stejný modul jako předkompilovaná verze.

### ZFS

[ZFS](https://openzfs.org/wiki/Main_Page) je jedním z mnoha souborových systémů, které jsou podporovány v CachyOS. Vzhledem k tomu, že je licencován pod
[CDDL](https://opensource.org/license/cddl-1-0), je nekompatibilní s licencí jádra Linuxu, a proto nemůže být začleněn do stromu jádra. Dodávaný modul obsahuje
nejnovější funkce a opravy z upstreamu, aby byla zajištěna kompatibilita s nejnovějším jádrem.

### NVIDIA

CachyOS dodává jak předkompilované verze uzavřených, tak [open-source](https://github.com/NVIDIA/open-gpu-kernel-modules/) modulů jádra. Vzhledem k tomu, že vývoj
modulu jádra NVIDIA probíhá mimo strom jádra, a proto nesleduje kadenci vydávání jádra, může být výchozí konfigurace někdy nekompatibilní s nejnovějším
jádrem. Jako řešení CachyOS záplatuje moduly záplatami vytvořenými komunitou nebo záplatami sdílenými přímo společností NVIDIA.

## Ostatní

Jádro CachyOS má také některé další pozoruhodné vlastnosti, které jsou nenápadné, ale zlepšují uživatelský komfort

- Zahrnuje ladicí variantu jádra, která poskytuje neodstraněný binární soubor jádra pro účely ladění. Tento balíček je potřebný k profilování jádra pomocí AutoFDO.
- [Binder](https://developer.android.com/reference/android/os/Binder), modul potřebný pro [Waydroid](https://waydro.id/), je ve výchozím nastavení povolen v konfiguraci jádra
a je již [nastaven](https://github.com/CachyOS/linux-cachyos/blob/master/linux-cachyos/config#L10559).

## Konvence pojmenování balíčků

```sh
linux-cachyos # Základní balíček jádra pro výchozí jádro. Kompilováno s clang
linux-cachyos-gcc # Protičást linux-cachyos kompilovaná s GCC
linux-cachyos-{,gcc-}headers # Hlavičky jádra, hlavně pro sestavování
linux-cachyos-{,gcc-}nvidia # Předkompilované uzavřené moduly NVIDIA pro jádro linux-cachyos
linux-cachyos-{,gcc-}nvidia-open
linux-cachyos-{,gcc-}zfs # Předkompilované moduly ZFS pro jádro linux-cachyos
linux-cachyos-{,gcc-}dbg # Neodstraněný binární soubor linuxu pro ladění

linux-cachyos-hardened # Základní balíček jádra pro hardened jádro. Kompilováno s GCC
linux-cachyos-hardened-lto # Protičást linux-cachyos-hardened kompilovaná s clang
linux-cachyos-hardened-{,lto-}headers
linux-cachyos-hardened-{,lto-}nvidia
linux-cachyos-hardened-{,lto-}nvidia-open
linux-cachyos-hardened-{,lto-}zfs
linux-cachyos-hardened-{,lto-}dbg
```

## FAQ

### Proč se AutoFDO nepoužívá pro všechny ostatní varianty jádra?

Protože je to drahé na sestavení, protože to v podstatě vyžaduje sestavení jádra dvakrát, a proto vyžaduje více prostředků a času věnovaného kompilaci. Proces sestavování jádra s AutoFDO zahrnuje následující kroky:

1)  Sestavte jádro s povolenými funkcemi AutoFDO a ladění.
2)  Vytvořte profil, což znamená provedení zátěžových testů za účelem shromáždění dat pro profilování pro možné optimalizace.
3)  Znovu sestavte jádro s profilem AutoFDO.

Proto je prozatím přítomno pouze ve variantě [linux-cachyos](https://www.google.com/url?sa=E&source=gmail&q=/features/kernel#linux-cachyos-výchozí-jádro).

Další informace o AutoFDO naleznete [zde.](https://cachyos.org/blog/2411-kernel-autofdo/)

### Zlepšuje jádro pro real-time výkon ve hrách?

Ne, nezlepšuje. Jádro pro real-time umožňuje preemptovat mnohem více kódu ve srovnání s normálním plně preemptivním jádrem. To znamená, že mnohem více úloh (včetně
herních procesů) je často preemptováno a bude nuceno uvolnit systémové prostředky, což vede k horšímu výkonu.

```
