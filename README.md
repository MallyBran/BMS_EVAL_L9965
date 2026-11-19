# BMS  ST L9965 s doskou NUCLEO-H723ZG

Cieľom projektu je oživiť BMS čipset L9965 od STMicroelectronics s riadiacou doskou NUCLEO-H723ZG. Výsledkom bude prototyp, ktorý meria napätia a teploty článkov a posiela ich do PC. Projekt slúži zároveň ako základ pre BMS formula student.

## Použitý hardvér

- **NUCLEO-H723ZG** – riadiace MCU, SPI master.
- **EVL9965T (L9965T)** – izolovaný SPI transceiver (dual-die, dvojkanálový, twisted-pair VIF). Most medzi logickou a izolovanou batériovou doménou.
- **EVL9965A (L9965A)** – 18-článkový battery monitor s balansovaním (400 mA/článok) + load board na emuláciu článkov pred zapojením skutočného 18S packu.
- **EVL9965C (L9965C)** - Coulomb counting,presny shunt, integracia prudu v case pre presne meranie SoC a SoH

Komunikácia beží v režime VIF daisy chain: **MCU → SPI → L9965T → izolovaný twisted pair -> L9965A**. Firmvér vychádza z balíka STSW-L9965FW od ST, ktorý má referenčnú implementáciu pre STM32L476, preto ho portujeme na H723.

## Úlohy členov tímu

### Harmady – základná konfigurácia a oživenie komunikácie

- Konfigurácia projektu v STM32CubeMX pre H723ZG: hodiny, SPI periféria (master, režim hodín podľa datasheetu L9965T), GPIO pre chip select a riadiace piny transceivera.
- Zapojenie Nucleo s EVL9965T a EVL9965A, kontrola napájania a logických úrovní.
- Wake-up sekvencia L9965T a L9965A a prvá úspešná komunikácia (vyčítanie identifikačného registra L9965A).
- Overenie SPI signálov logickým analyzátorom.
- 
- **Výstup:** funkčný základný projekt, na ktorom staviajú ostatní.
- 

### Mally – port firmvéru STSW-L9965FW na STM32H723

- Analýza štruktúry balíka – oddelenie platformovo nezávislej časti drivera od MCU-závislej vrstvy referenčnej implementácie pre STM32L476.
- Prepísanie MCU-závislej vrstvy na HAL knižnice H7: SPI prenos, ovládanie chip selectu a riadiacich pinov, časovanie (ms/µs oneskorenia, timeouty).
- Integrácia drivera do projektu (štruktúra súborov, include cesty) a ošetrenie rozdielov medzi L4 a H7 (SPI periféria, vyššia frekvencia jadra).
- Optional: Overenie funkcií drivera na reálnom hardvéri: konfigurácia L9965A, spustenie merania, vyčítanie napätí 18 článkov a teplôt.
- **Výstup:** funkčný driver na H723 s jednoduchým rozhraním (napr. funkcia, ktorá vráti napätia a teploty článkov).
- Meranie cez L9965C, data pre odhad SoC SoH

### Pusztai – prenos dát, GUI a logovanie

- Strana MCU: odosielanie nameraných dát cez UART (ST-LINK virtuálny COM port), návrh jednoduchého formátu správ (napätia článkov, teploty, stav systému), periodické odosielanie pomocou časovača.
- Strana PC: aplikácia na zobrazenie napätí a teplôt v reálnom čase a logovanie do súboru (CSV) na neskoršie vyhodnotenie.
- Spustenie oficialnej GUI od STM. Oficialne GUI bude prerobene a rozsirene o funkcionality.
- **Výstup:** živý prehľad stavu článkov na PC a záznam meraní z testov.
- 
