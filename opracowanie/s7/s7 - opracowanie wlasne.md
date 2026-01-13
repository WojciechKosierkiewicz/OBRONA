# Modele kompartmentowe procesów farmakokinetycznych

### Kilka definicji na start
**Proces farmakokinetyczny** - proces rozprzestrzeniania się leku w organizmie
**Stała eliminacji** - określa szybkość usuwania leku z organizmu i opisuje, jaka część leku jest eliminowana w jednostce czasu.
**Objętość dystrybucji** - hipotetyczna objętość, w której lek musiałby być równomiernie rozmieszczony, aby osiągnąć obserwowane stężenie we krwi
**Klirens** - określa objętość osocza całkowicie oczyszczaną z leku w jednostce czasu i jest miarą zdolności organizmu do jego eliminacji.
**Faza dystrybucji** - okres po podaniu leku, w którym następuje jego przemieszczanie się z krwi do tkanek i narządów.
**Faza eliminacj**i - okres, w którym stężenie leku zmniejsza się głównie na skutek metabolizmu i wydalania, po zakończeniu dystrybucji.
Perfuzja - oznacza przepływ krwi przez tkankę lub narząd, który determinuje tempo dostarczania i usuwania leku.

Modelowanie farmakokinetyczne służy do opisu zmian stężenia leku w organizmie w czasie, czyli procesów wchłaniania, dystrybucji i eliminacji. Najczęściej stosowanym podejściem są modele kompartmentowe, w których organizm opisuje się jako zbiór idealizowanych przedziałów (kompartmentów) o jednorodnym stężeniu leku. Bardziej zaawansowanym podejściem są modele fizjologiczne (perfuzyjne())
## Model Jednokompartmentowy

Model jednokompartmentowy zakłada, że cały organizm można traktować jako jeden kompartment, w którym lek po podaniu natychmiast i równomiernie się rozprasza. Eliminacja leku jest opisana jako proces proporcjonalny do jego ilości w organizmie, najczęściej z wykorzystaniem kinetyki pierwszego rzędu - model jest opisywany liniowym równaniem różniczkowym pierwszego rzędu.

Ze względu na swoje uproszczenia model jednokompartmentowy dobrze sprawdza się w przypadku leków, które szybko i równomiernie rozkładają się w organizmie, ale nie oddaje złożonej dystrybucji między narządami i tkankami.

Model jednokompartmentowy dobrze opisuje dystrybucję m.in. antybiotyków czy leków przeciwbólowych

Model jednokompartmentowy jest matematycznie prosty i pozwala opisać stężenie leku w czasie, wyznaczać parametry farmakokinetyczne, takie jak stała eliminacji, objętość dystrybucji czy klirens oraz analizować różne sposoby podania leku, np. wstrzyknięcie, wlew ciągły czy podaż pozakompartmentową

## Modele wielokompartmentowe

Modele wielokompartmentowe rozszerzają model jednokompartmentowy, zakładając istnienie kilku wzajemnie połączonych kompartmentów, pomiędzy którymi lek przemieszcza się z określonymi szybkościami. Zwykle wyróżnia się jakiś kompratment centralny i jeden lub więcej kompartmentów obwodowych. Te modele są układami sprzężonych równań różniczkowych 

Modele te pozwalają opisać osobno fazę dystrybucji i eliminacji, lepiej odwzorowują rzeczywiste krzywe stężenia leku w czasie oraz są częściej stosowane w praktyce klinicznej niż modele jednokompartmentowe.
Ich główną wadą jest większa liczba parametrów, co utrudnia identyfikację modeli i interpretację biologiczną

## Modele fizjologiczne (perfuzyjne)
Modele fizjologiczne, zwane również **perfuzyjnymi**, opierają się na rzeczywistej anatomii i fizjologii organizmu. Każdy istotny narząd lub grupa tkanek jest opisywana jako osobny kompartment, a transport leku pomiędzy nimi jest determinowany przez przepływ krwi oraz właściwości tkanek.

W modelach fizjologicznych:

- kompartmenty odpowiadają konkretnym narządom (np. wątroba, nerki, mózg),
- parametry mają bezpośrednie znaczenie biologiczne (przepływ krwi, objętość narządu),
- możliwe jest realistyczne modelowanie metabolizmu i eliminacji leku.

Modele te są najbardziej realistyczne, ale jednocześnie:
- najbardziej złożone matematycznie,
- wymagają dużej ilości danych fizjologicznych,
- są trudniejsze w kalibracji i obliczeniowo bardziej kosztowne.

Poza opisanymi wcześniej używa się również modeli hybrydowych, które są połączeniem podejścia kompartmentowego i perfuzyjnego oraz modeli farmakodynamicznych, które nie opisują stężenia leku, lecz jego efekt farmakologiczny jako funkcję stężenia.


Source:
https://lucc.pl/inf/informatyka_w_medycynie/wyklad/Modele_kompartmentowe.pdf
