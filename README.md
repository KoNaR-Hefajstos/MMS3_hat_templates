# Zestaw template'ów kompatybilnych z płytką MMS3 i systemem ChainBus

## Nie ma jeszcze standardu fizycznego płytki (czyli jeszcze nie musisz robić layout'u ) <p> Zostanie on dodany w późniejszym terminie (po sesji). Template schematic'owy jest już dostępny <p> Wymiary hat'a to około 100x50mm


### Komunikacja oraz sterowanie
<p> ChainBus jest w pełni cyfrowy. GPIO nie sterują bezpośrednio funkcjonalnością, tylko poprzez magistrale komunikacyjne i IC.
<p> Znaczy to, że zamiast używać GPIO do przełączania LED'a, odczytywania krańcówek czy zadawania PWM'a musisz zaimplementować jakiegoś IC który to robi. Np:
<p> MCU --> expander gpio po I2C --> Dioda LED
<p> MCU wybiera do którego hat'a się podpina przy użyciu bus switcha. To znaczy że magistrale I2C, SPI i UART są niezależne od siebie na każdym hat'ie (nie musisz się martwić o zajęte adresy I2C).
<p> Dodatkowo w celu identyfikacji każdego hat'a jest na nim pamięć EEPROM po I2C.

### Zasilanie
<p> Przez złącze ChainBus idzie następujące zasilanie

|    |      Napięcie      |  Prąd dla jednego hat'a |
|----------|:-------------:|------:|
| 5V |  5V | 125mA |
| 12V stby |    12V   |   65mA |
| BRD_VIN | Od 12V do 48V |    185mA |

<p> Hat'y powinny obsługiwać do 48V z BRD_VIN. Komponenty powinny być dostosowane aż do tego napięcia.

<p> Jeśli twoja płytka potrzebuje więcej mocy niż ile ChainBus daje, to można dodatkowo podłączyć Vin po XT60 która obsługuje około 60A.

<p> Jeśli potrzebujesz innych napięć to możesz użyć gotowej przetwonicy step-down z template'a. Można wybierać z niej  24V, 12V, 9V i 6V2. Wszystkie na 2A.


### Łączenie hat'ów
<p> Na jednym MMS3 można zamontować do 8 hatów. Łączy się przez wpinianie kolejnego MMS'a do złącza ChainBus wcześniejszego

### Przykładowy hat
[Sterownik silników krokowych. Komunikacja po SPI](https://github.com/KoNaR-Hefajstos/MMS3_hat_stepper_controler)
