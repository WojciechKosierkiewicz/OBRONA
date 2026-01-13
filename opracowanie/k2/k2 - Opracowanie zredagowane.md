# Przykłady systemów stałoprzecinkowych
## System naturalny
W systemie tym wartość kodowana przez reprezentację jest obliczana jako suma po wszystkich pozycjach iloczynów cyfr znajdującej się na danej pozycji oraz wagi charakterystycznej dla tej pozycji (Przykład: $5210.10_{N10} = 5\cdot10^{3}+2\cdot10^{2}+1\cdot10^{1}+0\cdot10^{0}+1\cdot10^{-1}+0\cdot10^{-2}$).
System naturalny koduje jedynie wartości nieujemne, z zakresem zależnym od liczby pozycji (np. dla N2: zakres $<0, 2^{k} -1>$ gdzie k to liczba pozycji)

## System znak - moduł
Względem systemu naturalnego pojawia się dodatkowy opcjonalny symbol (w rachunkach zazwyczaj oznaczany jako minus na początku prezentacji), wskazujący na fakt, że mamy do czynienia z reprezentacją kodującą ujemną wartość. Dla podstawy $\beta = 2$ n-elementowa reprezentacja koduje wartość z przedziału $<-(2^{n-1}+1), 2^{n-1}-1>$;
Ma dwie reprezentacje zera ($\pm 0$) - przykład kodu **redundantnego**.

# Arytmetyka zmiennoprzecinkowa
Wartość liczby obliczana jest ze wzoru na notację naukową $X = SM\beta^{W}$ gdzie:
- S - znak
- M bity ułamkowe mantysy (znormalizowane zgodnie z zakresem wynikającym z podstawy)
- $\beta$ - podstawa, baza liczbowa
- W- wykładnik (w implementacjach sprzętowych spolaryzowany, reprezentowany kodem z przesunięciem $\frac{2^{k}}{2}-1$)

## IEEE754

$X = (-1)^{S}\cdot M \cdot 2^{E - bias}$

Standard reprezentacji binarnej i operacji na liczbach zmiennoprzecinkowych, implementowany powszechnie w procesorach i oprogramowaniu obliczeniowym. Określa podział słowa kodowego na trzy pola bitowe:
- jeden bit znaku
- wykładnik z przesunięciem (w 32bit słowie long, bias = 127; w 64bitowym słowie double bias = 1023)
- część ułamkową mnożnika (mantysa)

W ramach standardu mieszczą się również elementy wspomagające sprzętową realizację arytmetyki zmiennoprzecinkowej, tj. wartości specjalne, wyjątki oraz tryby zaokrąglania.

### Wartości specjalne
- NaN - reprezentacje zwracane w momencie wykonania operacji nieprawidłowych dla argumentów, np: $\sqrt{x}$ dla  $x<0$
	- sNaN - sygnalizujące NaN, rozróżnienie dodane w procesorach rodziny x86; powoduje zgłoszenie wyjątku (X 1...1 0XXX...)
	- qNaN - ciche NaN, zwrócone w wyniku jedynie w celu propagacji błędu, nie zgłasza wyjątku (X 1...1 1XXX...)
- $\pm0$ - dwie reprezentacje, w których bit znaku jest dowolny a pozostałe mają wartość 0, w większości języków programowania operacja porównania jest tak przeciążona by $-0 == 0$ zwracało true (X 0...0 0000...)
- $\pm \infty$ - jest wynikiem operacji w przypadku przepełnienia (overflow), oraz przy dzieleniu przez 0 (X 1...1 000...)
- r. nieznormalizowana - underflow, tzn gdy wynik działania jest zbyt bliski 0 aby można było go zapisać z znormalizowaną mantysą, denormalizuje się wtedy mantysę

Zdenormalizowana mantysa ma wartość 0.XXX... zamiast normalnej 1.XXX..., sama normalizacja natomiast wynika z tego, że część całkowitą liczby zakodowanej binarnie można w domyśle pominąć (wiadomo, że zawsze będzie 1, zyskujemy w ten sposób dodatkowy bit na część ułamkową)

## Wyjątki FPU
- overflow - wynik działania nie mieści się w zakresie dopuszczalnym ze względu na maksymalne wartości wykładnika i mnożnika - wynikiem działania jest $\pm \infty$
- underflow - wynik działania jest zbyt bliski zera by mógł być reprezentowany z pełną dokładnością względną, wynikiem jest reprezentacja nieznormalizowana lub $\pm 0$
- inexact - wynik nie może być reprezentowany w sposób dokładny (np $\frac{1}{3}$) i musi zostać zaokrąglony. Zwracana wartość jest zależna od [[k2 - Opracowanie zredagowane#Tryby Zaokrąglania|trybu zaokrąglania]]
- invalid operation - wykonano operację, która jest dla danego argumentu niemożliwa, lub wynik jest pozbawiony sensu. Zwracana jest wartość NaN
- Division by zero - zwracana w przypadku, w którym doszło do dzielenia przez $\pm 0$. Wynikiem działania jest $\pm \infty$

## Tryby Zaokrąglania
- to nearest, ties to even
- to nearest, ties to 0
- toward 0
- toward $+ \infty$
- toward $-\infty$

| Tryb zaokrąglania \ Wartość | 2.5 | 1.5 | −2.5 |
| --------------------------- | --: | --: | ---: |
| To nearest, ties to even    |   2 |   2 |   −2 |
| To nearest, ties to 0       |   2 |   1 |   −2 |
| Toward 0                    |   2 |   1 |   −2 |
| Toward +∞                   |   3 |   2 |   −2 |
| Toward −∞                   |   2 |   1 |   −3 |
