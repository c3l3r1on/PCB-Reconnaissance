In the Name of Root
and Saint UART.

Ohmmmm, Brothers in the Circuit.

May the Resistor Be With You.
Burn, Dump, Repeat.
Glory to PCB and SMD.

For firmware is written,
but hardware is eternal.

And remember:

We Are All Grounds Here.

# TP-Link Archer C1200 v2 PCB-Reconnaissance

Zestaw zdjęć PCB użytych do diagnozy sprzętowej egzemplarza `TP-Link Archer C1200 v2`.

## Wesprzyj Lab
- Jeśli masz trochę martwego sprzętu, pół-martwego sprzętu albo sprzętu typu „zapalił się tylko raz”, to chętnie go przygarnę i przerobię na dokumentację.
- Kawą też nie pogardzę:
  - Ko-fi: <a class="link" href="https://ko-fi.com/c3l3r1on" target="_blank" rel="noopener">ko-fi.com/c3l3r1on</a>
  - Buy Me a Coffee: <a class="link" href="https://buymeacoffee.com/c3l3r1on" target="_blank" rel="noopener">buymeacoffee.com/c3l3r1on</a>

## Odczytane Oznaczenia Z PCB
- Główny układ switch Ethernet: `Broadcom BCM53125SKMMLG TE1734 P30 277-20 3B W`
- Główny SoC Broadcoma widoczny w oknie ekranu: `BCM47189B0KRFBG HN1737 P20 408-29 N3A W`
- Kwarc przy switchu: `Y1`, oznaczenie `25.0 S GL`
- Oznaczenia magnetyków Ethernet widoczne na zdjęciach: `HST-36001DR` `GROUP-TEK` `1744B`, `HST-18001DR` `GROUP-TEK` `1738`
- Oznaczenia PCB widoczne na płycie: `KB-6160` `1742` `A MKM` `94V-0` `10558` `E248237`
- Kod płytki / board code widoczny na płycie: `2050500863`

## Naklejki / Oznaczenia Montażowe
- Naklejka produktowa widoczna na zdjęciu od strony portów:
  - `Archer C1200(EU) Ver:2.0`
  - `2BHC184000074`
- Dodatkowa naklejka montażowa / produkcyjna:
  - `3FA4-03`

## Kontekst
- Firmware został przywrócony do stocka.
- Runtime i NVRAM wróciły do oczekiwanego mapowania LAN/WAN.
- Mimo tego switch nadal raportował fałszywy link na `port 0` nawet bez podpiętego kabla Ethernet.
- Diagnostyka zeszła z warstwy config/runtime na warstwę switch/PHY/zasilania.

## Przydatne Ustalenia Z UART
- CPU / platforma raportowana przez UART:
  - `ARMv7`
  - `Hardware: Northstar Prototype`
  - kernel `Linux 2.6.36.4brcmarm`
- Układ flash raportowany przez runtime:
  - `mtd0` `boot` `0x00080000`
  - `mtd1` `linux` `0x00f70000`
  - `mtd2` `rootfs` `0x00d70000`
  - `mtd3` `usb-config` `0x00010000`
  - `mtd4` `log` `0x00020000`
  - `mtd5` `nvram` `0x00010000`
- Analiza full flash / partition-table pokazała też vendorowe partycje poza prostym widokiem runtime MTD, m.in.:
  - `default-config`
  - `user-config`
  - `profile`
  - `product-info`
  - `soft-version`
  - `extra-para`
  - `qos-db`
  - `radio`
  - `radio_bk`
- Stockowe mapowanie sieci potwierdzone po UART po resecie:
  - `LAN = eth0.1`
  - `WAN = eth1.4094`
  - `br-lan = 192.168.0.1`
  - `sysmode = router`
- Najważniejszy wynik diagnostyczny:
  - po przywróceniu stockowej konfiguracji `et -i eth0 port_status all` nadal pokazywał `port 0 Up`
  - objaw utrzymywał się nawet bez podpiętego kabla RJ45
  - to był główny powód zejścia z analizy UCI/configu na hardware
- Ważna uwaga dla innych badaczy o factory reset:
  - w tym firmware reset nie oznacza „przepisz cały flash”
  - głównie przywraca `default-config -> user-config`
  - dane board-specific poza tą ścieżką mogą nadal zachowywać objawy sprzętowe
- Uwaga USB z UART:
  - `RTL8188EUS` (`0bda:8179`) był fizycznie wykrywany
  - stockowy firmware wiązał go z `KC NetUSB General Driver`, a nie z normalnym interfejsem Wi-Fi

## Trop Na Poziomie PCB
- Główny podejrzany rejon to okolica switcha Broadcoma oraz jego referencyjnego zegara `Y1`.
- Na zbliżeniach widać `Y1` z oznaczeniem `25.0 S GL`, co pasuje do kwarcu `25 MHz`.
- Pod mikroskopem sam kwarc i jego luty nie wykazywały oczywistego uszkodzenia mechanicznego.
- Późniejsze pomiary rezystancji pokazały bardzo niską rezystancję na badanej linii w tym obszarze, co jeszcze bardziej przesunęło diagnozę z samego configu na hardware.

## Zdjęcia

### `photos/photo02.jpg`
- Górna strona PCB, ujęcie ogólne.
- Dobrze pokazuje:
  - obszar portów RJ45 i magnetyków
  - główny radiator / rozpraszacz ciepła
  - sekcję zasilania po prawej
  - położenie przewodów serwisowych

### `photos/photo03.jpg`
- Dolna strona PCB, ujęcie ogólne.
- Przydatne do:
  - lokalizacji punktów pomiarowych
  - śledzenia przelotek i rozkładu zasilania
  - orientacji spodu względem obszaru radiatora/ekranu

### `photos/photo04.jpg`
- Zbliżenie na odsłonięty obszar Broadcoma po otwarciu ekranowanej sekcji.
- W kadrze widać:
  - główny układ switch / Ethernet
  - pobliskie pasywy
  - lokalne kondensatory i rezystory toru zegara/zasilania

### `photos/photo05.jpg`
- Drugie zbliżenie na obszar switcha Broadcoma.
- Wyraźnie pokazuje:
  - `BCM53125SKMMLG` `TE1734 P30` `277-20 3B W`
  - kwarc `Y1`
  - otaczające pasywy `R8`, `R102`, `C221`, `C222`
- To było jedno z kluczowych zdjęć do dalszych pomiarów.

### `photos/photo06.jpg`
- Zbliżenie na prawy dolny rejon PCB z przewodami serwisowymi.
- W kadrze widać:
  - obszar dolutowanych przewodów
  - pamięć SPI flash `HJ1742` `25Q127CSIG` `AP16207`

### `photos/photo07.jpg`
- Drugie ogólne ujęcie górnej strony PCB.
- Lepsze do szybkiego pokazania:
  - topologii płyty
  - położenia switcha Broadcom względem portów
  - sekcji zasilania, USB i wejścia DC

### `photos/photo08.jpg`
- Dolna strona PCB, nowsze i czystsze ujęcie niż wcześniejsze zdjęcie spodu.
- Przydatne do:
  - ogólnej dokumentacji spodu PCB
  - lokalizacji obrysu ekranu i skupisk pasywów
  - korelacji późniejszych zbliżeń z pełnym układem płyty

### `photos/photo09.jpg`
- Górna strona PCB z założoną metalową ramką radiatora/ekranu.
- Przydatne do:
  - pokazania położenia RAM, głównej logiki Broadcoma i okien w ekranie
  - udokumentowania stanu płyty używanego do późniejszych zbliżeń

### `photos/photo10.jpg`
- Zbliżenie na główny obszar logiki Broadcoma przez okno ekranu.
- Najbardziej przydatne do:
  - odczytu oznaczenia układu Broadcoma
  - udokumentowania nalotu / stanu powierzchni obudowy układu
  - `BCM47189B0KRFBG` `HN1737 P20` `408-29 N3A W`

### `photos/photo11.jpg`
- Zbliżenie na obszar pamięci NANYA.
- Przydatne do:
  - potwierdzenia oznaczenia kości RAM
  - dokumentacji pobliskiej cewki zasilania i lokalnej sieci pasywów
  - `NT5CC64M16GP-DI` `71905300EP L TW`

### `photos/photo12.jpg`
- Zbliżenie na górne okno ekranu z kwarcem i pobliskimi układami pomocniczymi/pasywami.
- Przydatne do:
  - dokumentacji rejonu zegara/reference clock powyżej głównego układu Broadcoma
  - korelacji tego obszaru z późniejszymi pomiarami

### `photos/photo13.jpg`
- Bardzo bliskie makro małego układu w górnym oknie ekranu.
- Zdjęcie jest bardziej miękkie niż pozostałe, ale nadal pomaga udokumentować:
  - położenie układu
  - przybliżone oznaczenie
  - otaczające pasywy w tym podobszarze
  - `888 633C 6744`

### `photos/photo14.jpg`
- Makro zbliżenie na mały obszar zasilania / elementów pomocniczych w pobliżu `U9`.
- Przydatne do:
  - dokumentacji lokalnej pomocniczej sekcji sprzętowej
  - korelacji późniejszych pomiarów rezystancji / zasilania z dokładnym położeniem elementów

### `photos/photo15.jpg`
- Zdjęcie strony z portami pokazujące naklejki produktowe i montażowe przy bloku złączy.
- Przydatne do:
  - zachowania dokładnych danych wersji sprzętowej ze stickerów
  - udokumentowania oznaczeń jednostkowych i produkcyjnych
  - powiązania sfotografowanej PCB z retailowym oznaczeniem modelu i wersji

### `photos/photo16.jpeg`
- Makro zbliżenie na switch `BCM53125SKMMLG` `TE1734 P30` `277-20 3B W` i kwarc `Y1`.
- Dobre zdjęcie poglądowe do publikacji, bo czytelnie pokazuje relację między:
  - switchem Broadcoma
  - `Y1 25.0`
  - pasywami toru zegara
  - magnetykami / obszarem portów w tle

### `photos/photo17.jpeg`
- Kadr z termowizji.
- Pokazuje hotspot w rejonie Ethernet / switch / front-edge port area.
- Zaznaczone maksimum na tym zdjęciu dochodzi do około `73.8 C`.
- To należy traktować jako trop diagnostyczny, a nie jako samodzielny dowód uszkodzenia konkretnego elementu.

## Uwagi Praktyczne
- Do pomiarów `pcb-resistance` najbardziej użyteczny okazał się rejon:
  - `BCM53125`
  - `Y1`
  - `C221/C222`
  - kondensatory odsprzęgające przy switchu
- Jeśli folder będzie publikowany, sensowna kolejność oglądania zdjęć to:
  1. top view
  2. bottom view
  3. switch close-up
  4. `Y1` close-up
  5. thermal frame

## Aktualny Wniosek Roboczy
- Zdjęcia wspierają diagnozę, że problem nie siedzi już tylko w firmware/configuration.
- Najmocniejszy trop to hardware w rejonie switch/PHY/zasilania, a nie błędna logiczna konfiguracja LAN/WAN.
