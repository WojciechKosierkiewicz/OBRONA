# Fizyczne nośniki danych – technologie, struktury i metody kodowania

Fizyczne nośniki danych służą do **trwałego zapisu informacji cyfrowej** i różnią się technologią zapisu oraz sposobem kodowania danych. Najczęściej stosowane są nośniki magnetyczne, półprzewodnikowe i optyczne, wykorzystujące odmienne zjawiska fizyczne.
## 1. Nośniki magnetyczne (HDD)

Dyski HDD to jedna z najstarszych technologii pamięci masowej, nadal szeroko stosowana w archiwizacji, serwerach i systemach Big Data ze względu na niski koszt za jednostkę pojemności.

**Struktura i zapis danych:**  
Dysk składa się z wirujących **talerzy** pokrytych warstwą ferromagnetyczną oraz **głowic odczytu/zapisu**, które unoszą się nad ich powierzchnią w odległości kilku nanometrów. Zapis polega na zmianie orientacji domen magnetycznych, a odczyt na detekcji zmian strumienia magnetycznego. Kontakt głowicy z talerzem powoduje awarię typu _head crash_.

**Organizacja i kodowanie:**  
Dane były historycznie adresowane przez **CHS**, obecnie przez **LBA**, gdzie nośnik widziany jest jako liniowa sekwencja bloków. Podstawową jednostką zapisu jest sektor (512 B lub 4096 B). Dane kodowane są jako zmiany magnetyzacji, z wykorzystaniem metod FM, MFM oraz współczesnych technik **RLL**, które zwiększają gęstość zapisu i poprawiają synchronizację odczytu.
## 2. Nośniki półprzewodnikowe (SSD)

Dyski SSD nie posiadają części ruchomych i wykorzystują pamięci **Flash NAND**, co zapewnia bardzo krótki czas dostępu i wysoką odporność mechaniczną.

**Budowa i zapis:**  
Podstawowym elementem jest tranzystor z **bramką pływającą**, w której gromadzony ładunek elektryczny określa wartość logiczną. Zapis i kasowanie wymagają wysokich napięć, co powoduje stopniowe zużywanie komórek.

**Typy komórek (gęstość zapisu):**

- **SLC** – 1 bit, najwyższa trwałość i wydajność, zastosowania serwerowe
- **MLC** – 2 bity, kompromis trwałości i pojemności
- **TLC** – 3 bity, standard konsumencki
- **QLC** – 4 bity, wysoka pojemność kosztem trwałości

**Organizacja pamięci:**  
Dostęp do danych odbywa się blokami i stronami, co wymaga stosowania mechanizmów takich jak **wear leveling** i **garbage collection**, realizowanych przez kontroler SSD.

- **Wear leveling** – mechanizm równomiernego rozkładania operacji zapisu pomiędzy wszystkie komórki pamięci, aby zapobiec ich przedwczesnemu zużyciu.
- **Garbage collection** – proces porządkowania danych polegający na przenoszeniu aktualnych danych i zwalnianiu bloków zawierających nieaktualne informacje, aby przygotować je do ponownego zapisu.
## 3. Nośniki optyczne (CD, DVD, Blu-ray)

Nośniki optyczne zapisują dane w postaci **pitów i landów**, a odczyt odbywa się poprzez analizę odbicia wiązki lasera. Informacja binarna kodowana jest jako **zmiana pomiędzy pitem a landem**, a nie ich bezwzględna obecność.

Zwiększanie pojemności uzyskano dzięki skracaniu długości fali lasera i zmniejszaniu odstępów między ścieżkami (CD → DVD → Blu-ray).

**Kodowanie kanałowe:**  
W płytach CD stosuje się **EFM**, a w DVD **EFMPlus**, które zapewniają poprawną synchronizację i niezawodny odczyt danych.