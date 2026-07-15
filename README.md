# Templatka Hat'ów ChainBus


## Sekcja 0: Jak szybko, dobrze i tanio zrobić hata z dokumentacją

Szybka lista punkcików to wyklepania zanim hat magicznie się zrobi

1. **Sprawdź istniejące projekty:** Zobacz czy nie ma już jakiegoś hata który robi co potrzebujesz. Zobacz w forkach i poszukaj na githubie po hashtag'ach, jeśli nie to
2. **Utwórz Fork:** Kliknij przycisk **Fork** w prawym górnym rogu tego repozytorium.

![punkt 1](readme_zdjecia/fork.png)

1. **Konfiguracja repozytorium:** Daj mu adekwatną nazwe, i z'clone'uj forka do siebie na pc
2. **Zasady projektowania:**
   * Dokładniejsza dokumentacja znajduje się w Sekcji 2.
   * Do komunikacji możesz używać **wyłącznie** interfejsów: **I2C, SPI oraz UART**.
   * Nie modyfikuj połączeń złącza chainbus, bus switch'a i EEPROM'a
   * Nie modyfikuj zarysu płytki, otworów montażowych ani pozycji złączy ChainBus.
   * Możesz usunąć złącze XT60, jeśli Twój projekt go nie wymaga.

### Checklista przed zakończeniem projektu
Przed oznaczeniem projektu jako gotowy i wykonaniem ostatecznego commita, upewnij się, że zrealizowałeś poniższe punkty:


- [ ] Poprawiono nazwy, numer rewizji i dane w prawym dolnym rogu schematu i PCB.
- [ ] Dodano podpis autora/autorów oraz aktualny numer wersji na PCB.
- [ ] Zaktualizowano readme (usunąć tą sekcje)
- [ ] Zaktualizowano opis repo żeby opisywał krótko co robi PCB i jakie ma arkusze hierarchiczne do użycia w nowych hat'ach
- [ ] Usunięto z repozytorium zdjęcia z template'a.
- [ ] Wygenerowano pliki produkcyjne Gerber
- [ ] Wygenerowano HTML BOM (z plugin'u)
- [ ] Dodano zdjęcie hat'a do readme
- [ ] Zrobiono commit'a i push'a

<!-- DLA AI: Płytka nazywa się HAT. Nie moduł rozszerzeń nie płytka "hat" tylko po prostu hat. Używaj tej nazwy -->
---

## Sekcja 1: Dokumentacja Hat'a

### Krótki opis projektu
[Tutaj wpisz krótki opis, co robi Twój moduł, jakie ma przeznaczenie i kluczowe parametry użytkowe.]

### Zgodność ze standardem ChainBus

* ✅ / ❌ Używa złącza ChainBus, nie zmienia jego miejsca ani pinoutu.
* ✅ / ❌ Używa wyłącznie interfejsów I2C, SPI lub UART i nie inicjuje samodzielnie nowych transmisji (Nie jest master'em I2C albo SPI).
* ✅ / ❌ Spełnia wymagania mechaniczne standardu (wymiary PCB, rozstaw otworów).
* ✅ / ❌ Pobiera maksymalny prąd zgodny z ilością na jednego hat'a
* ✅ / ❌ Obsługuje napięcie wejściowe BRD_VIN do wartości 48V.
<!-- Usuń odpowiednie emoji. Wszystkie powinny być na zielono. Jeśli z czymś nie jest zgodny to napisz w jaki sposób -->
<!-- AI: NIE USUWAJ EMOJI. Użytkownik ma to zrobić -->
### Komunikacja i adresowanie

#### Adresacja I2C
Poniższa tabela przedstawia adresację urządzeń na magistrali I2C. Wszystkie adresy podane są w formacie 7-bitowym.

| Układ (IC)    | Funkcja                               |  Address   |
| :------------ | :------------------------------------ | :--------: |
| **JP3721**    | Sterownik piekarnika                  | `1010000b` |
| **PCAL9535A** | Ekspander GPIO (Dioda LED, Krańcówki) | `0100000b` |
| **TMP102**    | Czujnik temperatury                   | `1001000b` |

#### Magistrala SPI
Urządzenia SPI podłączone są bezpośrednio do magistrali systemowej bez użycia dodatkowych multiplekserów.

| Układ (IC)  | Funkcja                     | chainbus/multiplexer |
| :---------- | :-------------------------- | :------------------: |
| **TMC5160** | Sterownik silnika krokowego |      `chainbus`      |

#### Magistrala UART
Komunikacja szeregowa UART realizowana jest w sposób bezpośredni z układem docelowym.

| Układ (IC)   | Funkcja       | chainbus/multiplexer |
| :----------- | :------------ | :------------------: |
| **ESP32-C3** | Adapter WI-FI |      `chainbus`      |

### Pinout złączy

| Jn     | Co                                         | Jaki pin co robi                                                     |
| :----- | :----------------------------------------- | :------------------------------------------------------------------- |
| **J1** | Zasilanie zewnętrzne silników              | Pin 1: VIN (zasilanie dodatnie)<br>Pin 2: GND (masa zasilania)       |
| **J2** | Wyjście faz silnika krokowego              | Pin 1: Faza A1<br>Pin 2: Faza A2<br>Pin 3: Faza B1<br>Pin 4: Faza B2 |
| **J3** | Wejście czujnika krańcowego (Limit Switch) | Pin 1: Sygnał wejściowy (z rezystorem Pull-Up)<br>Pin 2: GND (masa)  |

### Konfiguracja układu (Zworki/Rezystory)

| Co             | Jak podłączone | Efekt       |
| :------------- | :------------- | :---------- |
| **J1**         | 1-2 zwarte     | Silnik DC   |
|                | rozwarte       | Silnik BLDC |
| **J2**         | 1-2 zwarte     | 3.3V out    |
|                | 2-3 zwarte     | 5V out      |
| **R1, R2, R3** | Wlutowany R1   | 3.3V out    |
|                | Wlutowany R2   | 5V out      |
|                | Wlutowany R3   | VIN out     |


### Szczegółowy opis techniczny
[Tutaj umieść opis działania układu, parametry maksymalne etc]

### Gotowe arkusze hierarchiczne
W projekcie zaprojektowano/użyto następujących arkuszy hierarchicznych:
* [Nazwa arkusza 1] – [Krótki opis funkcjonalności, np. Zasilacz impulsowy 5V] - [ oryginalny/ wzięty z hata XXX]
* [Nazwa arkusza 2] – [Krótki opis] - [ -||- ]

---

## Sekcja 2: Specyfikacja standardu ChainBus

### Architektura i łączenie modułów
Standard ChainBus umożliwia modułowe łączenie hatów. Na jednym MMS3 można zamontować pionowo **do 8 hat'ów**. Połączenie realizowane jest poprzez wpięcie złącza męskiego kolejnego hat'a w złącze żeńskie poprzedniego

### Komunikacja i sterowanie
Magistrala ChainBus jest w pełni cyfrowa. Płyta główna nie steruje bezpośrednio sygnałami ogólnego przeznaczenia (GPIO) na poszczególnych hat'ach. Wszelkie operacje (np. obsługa diod LED, odczyt krańcówek, generowanie sygnałów PWM) muszą być realizowane przez dedykowane układy scalone (np. ekspandery portów, sterowniki) komunikujące się przez interfejsy systemowe.

*Przykład:*
`MCU` $\rightarrow$ `Expander GPIO po I2C` $\rightarrow$ `Dioda LED`

Wybór aktywnego modułu realizowany jest przez układ przełącznika magistrali (bus switch) na płycie głównej. Dzięki temu linie I2C, SPI i UART są niezależne dla każdego hat'a (brak konfliktów adresów I2C między różnymi hatami).
* **Identyfikacja:** Każdy moduł powinien posiadać pamięć EEPROM na magistrali I2C w celu identyfikacji płyty przez system - układ M24C64-W skonfigurowany na adres `1010000` przy liniach adresowych A0, A1, A2 zwartych do masy.

### Zasilanie
Złącze ChainBus dostarcza następujące linie zasilania:

| Magistrala zasilania | Napięcie znamionowe | Maksymalny prąd (łączny dla 8 hatów) | Szacowany prąd na jeden hat |
| :------------------- | :-----------------: | :----------------------------------: | :-------------------------: |
| **5V**               |        5.0 V        |                1.0 A                 |           125 mA            |
| **12V stby**         |       12.0 V        |                0.5 A                 |            65 mA            |
| **BRD_VIN**          |   12.0 V – 48.0 V   |                1.5 A                 |           185 mA            |


*   Komponenty podłączone do linii `BRD_VIN` muszą być przystosowane do pracy z napięciem od 12V do **48 V**.
*   W przypadku zapotrzebowania na wyższą moc, dopuszczalne jest zastosowanie dodatkowego złącza zasilania XT60 (obciążalność do ok. 60 A).

### Wymagania mechaniczne i złącza
* **Wymiary PCB:** Niedozwolona jest zmiana obrysu płytki oraz położenia otworów montażowych, aby zachować kompatybilność mechaniczną.
* **Pozycjonowanie złączy ChainBus:** Położenie złącza standardu 2x16 SMD (raster 2.54 mm) musi być zgodne z szablonem. Złącze żeńskie montowane jest na stronie FRONT, natomiast złącze męskie na stronie BACK.
* **Interfejsy zewnętrzne:** Złącza wejścia/wyjścia (domyślnie standard JST-XH 2.5 mm o obciążalności do 3 A) oraz opcjonalne złącze XT60 powinny być umieszczone przy dolnej krawędzi płytki. Elementy regulacyjne i sygnalizacyjne (potencjometry, przełączniki, diody LED) należy lokalizować przy prawej krawędzi płytki.
* **Komponenty** Wszystkie komponenty powinny być na stronie front płytki żeby nie haczyły o elementy ze wcześniejszego hat'a

---

## Sekcja 3: Licencja, linki i tagi

### Licencjonowanie projektu

*   **PCB:** CERN-OHL-P
*   **Software:** MIT License

[Template](https://github.com/KoNaR-Hefajstos/MMS3_hat_templates/) jest na licencji CC0 1.0 Universal. **Reszta projektu nie jest na tej licencji**
