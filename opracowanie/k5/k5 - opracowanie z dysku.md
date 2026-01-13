# Podstawowe definicje
- problem obliczeniowy - zadanie do wykonania, wraz z danymi wejściowymi oraz warunkami jakie ma spełnić wynik
- instancja problemu - problem obliczeniowy z konkretnymi danymi wejściowymi
- algorytm - ciąg czynności koniecznych do wykonania w celu rozwiązania problemu. Algorytm musi zapewniać wynik zgodny z warunkami zdefiniowanymi w problemie
- złożoność obliczeniowa - ilość zasobów potrzebnych do rozwiązania problemu

# Ocena złożoności algorytmów.

Ocena złożoności algorytmów to szacowanie ilości zasobów (czasu procesora i pamięci RAM), jakie algorytm zużyje w funkcji rozmiaru danych wejściowych ($n$). Używamy do tego notacji asymptotycznej (głównie **Big O**), aby określić, jak szybko rosną wymagania algorytmu, gdy $n$ dąży do nieskończoności.​

---
## 1. Dwa rodzaje złożoności

Warto podkreślić, że "złożoność" to nie tylko czas.
- **Złożoność czasowa ($T(n)$):** Liczba operacji dominujących (podstawowych, np. porównań, przypisań) wykonywanych przez algorytm. To najczęstszy temat pytań.
  
  *Alternatywnie: określa w jakim tempie rośnie ilość czasu potrzebna do wykonania problemu, w zależności od rozmiaru problemu (ilości danych wejściowych). Przyjętą miarą jest ilość operacji dominujących (podstawowych)*
- **Złożoność pamięciowa ($S(n)$):** Ilość pamięci operacyjnej potrzebnej do wykonania zadania (ponad sam rozmiar danych wejściowych).
  
  *alt: w jakim tempie rośnie ilość pamięci potrzebna do przechowania wszystkich struktur danych wymaganych przez algorytm, w zależności od rozmiaru problemu*
    - _Przykład praktyczny:_ Jeśli wczytujesz cały plik logów do RAM-u, masz złożoność pamięciową $O(n)$. Jeśli przetwarzasz go linijka po linijce (stream), masz $O(1)$. To kluczowe przy dużych systemach.


Porównując złożoność obliczeniową algorytmów bierze się pod uwagę **asymptotyczne tempo wzrostu**, czyli to, jak zachowuje się funkcja określająca złożoność dla odpowiednio dużych, granicznych rozmiarów danych wejściowych.
## 2. Big O Notation - Pesymistyczny przypadek (Górna granica)

Notacja ta opisuje **pesymistyczny przypadek** (worst-case scenario) – czyli górne ograniczenie wzrostu funkcji:

| Notacja           | Nazwa                 | Przykład praktyczny                                                                                                                  |
| ----------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **$O(1)$**        | Stała                 | Dostęp do elementu tablicy przez indeks; sprawdzenie flagi uprawnień użytkownika w HashMapie. Niezależne od liczby danych​.          |
| **$O(\log n)$**   | Logarytmiczna         | Wyszukiwanie binarne; operacje na drzewach BST (np. szukanie commita w historii Gita, jeśli jest dobrze zindeksowana).               |
| **$O(n)$**        | Liniowa               | Przejście pętlą przez listę recenzentów; znalezienie max wartości w nieposortowanej tablicy. Czas rośnie proporcjonalnie do danych​. |
| **$O(n \log n)$** | Liniowo-logarytmiczna | Dobre algorytmy sortowania (Merge Sort, Quick Sort - średnio, TimSort w Javie).                                                      |
| **$O(n^2)$**      | Kwadratowa            | Zagnieżdżone pętle (np. porównanie każdego pliku z każdym w celu znalezienia duplikatów).​                                           |
| **$O(2^n)$**      | Wykładnicza           | Rekurencja bez zapamiętywania (np. naiwny Fibonacci). Zabójcza dla wydajności przy $n > 30-40$.                                      |
| $O(n!)$           | Silnia                | Zbiór wszystkich permutacji zbioru, lub brute force komiwojażera                                                                     |
## 3. Big Omega ($\Omega$) – Optymistyczny przypadek (Dolna granica)

Opisuje "najlepszy możliwy" scenariusz wykonania algorytmu lub dolną granicę trudności problemu.
- **Definicja:** Funkcja $f(n)$ jest $\Omega(g(n))$, jeśli dla dużych $n$, algorytm wykonuje **co najmniej** $c \cdot g(n)$ operacji.​
- **Co to znaczy dla inżyniera:** Mówi nam, że "niezależnie od tego, jak dobre dane dostaniemy, algorytm nie będzie szybszy niż to".
- **Przykład (Sortowanie):** Dla algorytmu sortowania przez wstawianie (Insertion Sort), jeśli tablica jest już posortowana, pętla wewnętrzna nigdy się nie wykona. Czas wykonania to $\Omega(n)$ (tylko jedno przejście, by sprawdzić, czy jest posortowana).
    - _Uwaga:_ Mimo że najlepszy przypadek to $\Omega(n)$, pesymistyczny (Big O) to nadal $O(n^2)$. Dlatego $\Omega$ rzadko służy do doboru algorytmu (bo musimy być gotowi na najgorsze).​

## 4. Big Theta ($\Theta$) – Dokładne oszacowanie (Przypadek przeciętny/ścisły)

Używamy jej, gdy algorytm w pesymistycznym i optymistycznym przypadku zachowuje się tak samo (rzędy wielkości się pokrywają).
- **Definicja:** Funkcja $f(n)$ jest $\Theta(g(n))$, jeśli jest ograniczona z góry i z dołu przez tę samą funkcję $g(n)$ (z dokładnością do stałych). Inaczej: $f(n)$ rośnie **w takim samym tempie** jak $g(n)$.
- **Kiedy używać na obronie:** Gdy jesteś pewien, że algorytm zawsze działa tak samo.
- **Przykład (Przejście przez tablicę):** Jeśli iterujesz przez listę recenzentów, żeby wyświetlić ich avatary, _zawsze_ musisz przejść przez wszystkich $n$ recenzentów. Nie ma znaczenia, czy są posortowani, czy nie.
    - Big O: $O(n)$
    - Big Omega: $\Omega(n)$
    - Wniosek: Złożoność wynosi dokładnie $\Theta(n)$ (Theta od n).
## 5. Sposoby oceny (Jak to liczymy?)

- **Metoda teoretyczna (analityczna):** Analiza kodu źródłowego. Zliczamy zagnieżdżenia pętli.
    - Jedna pętla `for(i=0; i<n; i++)` -> $O(n)$.
    - Pętla w pętli -> $O(n^2)$.
    - Dzielenie problemu na pół w pętli (np. `while(i > 0) i /= 2`) -> $O(\log n)$.
- **Metoda eksperymentalna (empiryczna):** Uruchomienie kodu dla różnych wielkości danych ($n=100, 1000, 10000$) i pomiar czasu ("benchmark"). W Javie używa się do tego narzędzi takich jak **JMH (Java Microbenchmark Harness)**, aby uniknąć błędów pomiarowych związanych z rozgrzewaniem JVM i JIT Compilerem.

## 7. Klasy problemów (P vs NP)

Może paść pytanie o to, czy każdy problem da się rozwiązać szybko.
- **Klasa P:** Problemy **decyzyjne** (ważne), które da się rozwiązać w czasie wielomianowym ($O(n^k)$) (np. sortowanie, szukanie ścieżki).
- **Klasa NP:** Problemy **decyzyjne**, dla których poprawność rozwiązania da się **zweryfikować** w czasie wielomianowym (ale znalezienie rozwiązania może trwać wieki, np. łamanie szyfrów, problem komiwojażera).
- **Klasa NP-trudne**: problemy, które są co najmniej tak trudne jak każdy problem NP, czyli każdy problem z NP można do nich sprowadzić w czasie wielomianowym. Nie muszą należeć do NP - mogą przykładowo nie być problemami decyzyjnymi lub nie mieć wielomianowego algorytmu weryfikacji rozwiązania
- **Klasa NP-zupełne**: Problemy, które należą do klasy NP i są NP - trudne. Oznacza to najtrudniejsze problemy z klasy NP. Znalezienie wielomianowego algorytmu dla dowolnego problemu NP - zupełnego implikowałoby, że wszystkie problemy z klasy NP można rozwiązać w czasie wielomianowym.

Dobrze jest podkreślić że P i NP są decyzyjne, komisja może wtedy zapytać czemu akurat decyzyjne (albo możesz sam powiedzieć, jak ci zostanie czasu i poczujesz potrzebę mówienia) i najlepiej odpowiedzieć że:

Ponieważ opierają się na formalnym modelu obliczeń, w którym złożoność czasu jest jednoznacznie mierzona, a relacje między problemami są opisywane za pomocą redukcji wielomianowych. 
Ponadto, problemy decyzyjne mają binarną odpowiedź TAK/NIE, co umożliwia porównywanie ich trudności oraz formalne definiowanie pojęć takich jak NP-trudność i NP-zupełność.
Problemy optymalizacyjne lub wyszukiwawcze można zwykle sprowadzić do problemów decyzyjnych, dlatego analiza złożoności koncentruje się na tej postaci problemu. (optymalizacyjny komiwojażer *jaka jest najkrótsza ścieżka?* może być przedstawiony jako *czy istnieje trasa krótsza niż X?*)