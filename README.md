# Zestaw template'ów kompatybilnych z płytką MMS3 i systemem ChainBus

### Tworzenie własnego hata
<p> Żeby stworzyć własnego hata musisz zrobić fork'a tego repo i na nim zaimplementować to co potrzebujesz
<p> 0. Zobacz na fork'i czy ktoś już nie zrobił płytki której potrzebujesz, jeśli nie to
<p> 1. Wejdź na github'a template'a i kliknij "fork"

![punkt 1](readme_zdjecia/fork.png)
<p>2. Zmień nazwe i zrób fork'a
<p>3. Sklonuj repo swojego właśnie zrobionego forka
<p>4. Usuń niepotrzebne pliki z template'a (zdjęcia), zaktualizuj readme, zmień nazwy plików .kicad z MMS_hat_templates na zgodną z twoim projektem i zmień nazwy w tabelce w prawym dolnym rogu
<p>5. Gotowe, możesz push'ować nowe commity do swojego fork'a

### Łączenie hat'ów
<p> Na jednym MMS3 można zamontować do 8 hatów. Łączy się przez wpinianie kolejnego Hat'a do złącza ChainBus wcześniejszego

### Komunikacja oraz sterowanie
<p> ChainBus jest w pełni cyfrowy. GPIO nie sterują bezpośrednio funkcjonalnością, tylko poprzez magistrale komunikacyjne i IC.
<p> Znaczy to, że zamiast używać GPIO do przełączania LED'a, odczytywania krańcówek czy zadawania PWM'a musisz zaimplementować jakiegoś IC który to robi. Np:
<p> MCU --> expander gpio po I2C --> Dioda LED
<p> MCU wybiera do którego hat'a się podpina przy użyciu bus switcha. To znaczy że magistrale I2C, SPI i UART są niezależne od siebie na każdym hat'ie (nie musisz się martwić o zajęte adresy I2C). Ale musisz pamiętać żeby podpinać sie z busa Chainbus a nie Chainbus_IN (czyli tego za bus swichem)
<p> Dodatkowo w celu identyfikacji każdego hat'a jest na nim pamięć EEPROM po I2C. Możesz użyć np. M24C64-W z obudową SOIC-8 i addresie 1010000 (przy A0 A1 A2 zwartych do GND)

### Zasilanie
<p> Przez złącze ChainBus idzie następujące zasilanie

|          |   Napięcie    | Prąd na wszystkie haty | Prąd dla jednego hat'a |
| -------- | :-----------: | :--------------------: | ---------------------: |
| 5V       |      5V       |           1A           |                  125mA |
| 12V stby |      12V      |          0.5A          |                   65mA |
| BRD_VIN  | Od 12V do 48V |          1.5A          |                  185mA |

<p> Prąd dla jednego hat'a został policzony w wypadku kiedy wszystkie 8 hat'ów jest zamontowanych. Jeśli na jeden MMS zamontujesz mniej hat'ów to każdy może pociągnąć więcej prądu.

<p> Hat'y powinny obsługiwać do 48V z BRD_VIN. Komponenty powinny być dostosowane aż do tego napięcia.

<p> Jeśli twoja płytka potrzebuje więcej mocy niż ile ChainBus daje, to można dodatkowo podłączyć Vin po XT60 która obsługuje około 60A.

### Template Mechaniczny
<p> Nie możesz zmieniać wielkości PCB, rozstawienia otworów montażowych oraz pozycji złącz chainbus (02x16 SMD, 2.54 raster) żeby płytka była kompatybilna z ChainBus'em
<p> Na płytce jest też złączka XT-60, ale jeśli jej nie potrzebujesz to możesz ją usunąć

### Wyprowadzenia
<p> Po środku płytki na górze jest złącze Chainbus. Na od front'u  jest żeńskie, na Back'u jest męskie.
<p> Na dolnej krawędzi płytki powinny być umieszczone złącza wyjścia/wejścia i złacza XT60 jeśli ich używasz. Na prawej wszystkie elementy konfiguracyjne i debugowe (potencjometry, przyciski, LED'y).




<p> Domyślnie jako złącz używamy JST-XH 2.5mm. Producent mówi że obsługują do 3A

### Przykładowy hat
[Sterownik silników krokowych. Komunikacja po SPI](https://github.com/KoNaR-Hefajstos/MMS3_hat_stepper_controler)
