
# Generowanie realistycznych obrazów scen 3-D za pomocą metody śledzenia promieni.

## 1. Generowanie obrazu (Wprowadzenie)

Ray Tracing to technika z rodziny **Global Illumination** (Oświetlenie Globalne). W przeciwieństwie do modeli lokalnych, jak rasteryzacja, Ray Tracing uwzględnia **wzajemne oddziaływanie optyczne** pomiędzy obiektami w scenie. Algorytm w dużym skrócie polega na wyznaczaniu koloru piksela na podstawie rekursywnego śledzenia ścieżek promieni.

**Kluczowa różnica:**
- **Rasteryzacja (Klasyczna):** Rzutujemy trójkąty na ekran (Object-order). Szybka, ale słaba w odbiciach.
- **Ray Tracing:** Dla każdego piksela szukamy obiektu w świecie (Image-order). Wolny, ale fotorealistyczny.

---

## 2. Algorytm Śledzenia Promieni (Krok po kroku)
Proces dla pojedynczego piksela $(x,y)$:
1. **Generacja promienia:** Z punktu w którym znajduje się obserwator wyprowadzany jest promień pierwotny, który przecina rzutnię (w punkcie $(x,y)$).
2. **Szukanie przecięcia (Intersection):** Wyszukiwany jest najbliższy punkt przecięcia z obiektami znajdujacymi się na scenie, żeby nie przeszukiwać wszystkich możliwych obiektów i uprościć ilość obliczeń stosuje się **struktury podziału przestrzeni**: Najlepszym przykładem takiej struktury jest **drzewo brył ograniczających (BVH)**. 
	- na początku grupuje się obiekty w duże, prostsze bryły (np sześciany). 
	- pozwala to najpierw sprawdzić czy promień w ogóle trafia w dane miejsce na scenie, 
	- jeśli nie od razu możemy zignorować wszystkie obiekty w środku, 
	- w przeciwnym wypadku skupiamy się na zawartości bryły.
	![[BVH.png]]
		(Jakby się chcieć bardzo spocić można dodać że skraca to złożoność przeszukiwania sceny z O(n) do O(log n))
3. **Obliczenie koloru (Shading):** Dla znalezionego punktu obliczany jest wpływ źródeł światła (model Phonga/Lamberta). W tym kroku wysyłane są tzw. **promienie cienia (shadow rays)** do źródeł światła, aby sprawdzić, czy punkt nie jest zasłonięty.
4. **Rekurencja (Promienie wtórne):** Jeśli obiekt jest lustrzany lub przeźroczysty, generowane są promienie odbite/załamane. Algorytm powtarza dla nich kroki 2-4, a wynikowy kolor jest sumowany z kolorem obliczonym w kroku 3
5. **Stop:** Śledzenie promienia dla danej ścieżki kończy się, gdy:
	- promień nie trafi w żaden obiekt (ucieczka w tło),
	- zostanie osiągnięta maksymalna liczba odbić (zabezpieczenie przed pętlą nieskończoną),
	- trafimy na obiekt matowy (który nie generuje promieni lustrzanych/przeźroczystych),
	- wystąpi całkowite wewnętrzne odbicie (wtedy nie generujemy promienia załamanego, co kończy tę konkretną gałąź).

---
## 3. Model Oświetlenia Phonga (Phong Lighting Model)
Służy do obliczenia koloru punktu na powierzchni, uwzględniając materiał i światło. Składa się z sumy trzech wektorów/komponentów:
I=Iambient+Idiffuse+IspecularI
1. **Ambient (Otoczenie):** Stały kolor, "rozjaśnia cienie", symuluje światło rozproszone wielokrotnie odbite, którego nie liczymy dokładnie.
    - _Wzór:_ $k_a \cdot i_a$
2. **Diffuse (Rozproszenie - Lamberta):** Światło matowe. Zależy od kąta padania światła. Powierzchnia jest najjaśniejsza, gdy światło pada prostopadle.
    - _Wzór:_ $k_d \cdot i_d \cdot (N \cdot L)$ (gdzie $N$ to normalna, $L$ to wektor do światła, a $\cdot$ to iloczyn skalarny, czyli cosinus kąta).
3. **Specular (Odblask lustrzany):** Biała plamka światła na błyszczącej kuli. Zależy od kąta obserwacji (widzisz odblask tylko gdy patrzysz pod odpowiednim kątem).
    - _Wzór:_ $k_s \cdot i_s \cdot (R \cdot V)^n$ (gdzie $R$ to wektor odbity, $V$ to wektor do oka, a $n$ to "shininess" - im większe, tym mniejsza i ostrzejsza plamka).[](http://mchwesiuk.pl/wp-content/uploads/2019/04/Grafika-Komputerowa-W5.pdf)​

---

## 4. Metody Cieniowania (Interpolacja) – Gouraud vs Phong
Częste pytanie o różnicę między modelem oświetlenia a metodą cieniowania (interpolacji):[](https://wp.faculty.wmi.amu.edu.pl/GRKW4.pdf)​
- **Cieniowanie Gourauda (Interpolacja koloru):**
    - Liczysz model oświetlenia (wzór Phonga) **tylko w wierzchołkach** trójkąta.
    - Kolor wnętrza trójkąta jest uśredniany (interpolowany liniowo).
    - _Zaleta:_ Szybkie.
    - _Wada:_ "Gubi" odblaski (specular) wewnątrz trójkąta. Wygląda kanciasto (efekt pasm Macha).
- **Cieniowanie Phonga (Interpolacja wektora normalnego):**
    - Interpolujesz **wektor normalny** dla każdego piksela wewnątrz trójkąta.
    - Liczysz model oświetlenia (wzór Phonga) **dla każdego piksela osobno**.
    - _Zaleta:_ Idealnie gładkie odblaski.
    - _Wada:_ Wolniejsze (ale w dobie GPU to standard).


Śledzenie promieni nie jest jednak metodą idealną. Duża ilość obliczeń wymaga znacznych zasobów komputera, przez co jeszcze niedawno rzeczywiste wykorzystanie tego algorytmu nie było możliwe, poza sprzętem specjalistycznym. Generowanie pełnego obrazu wymaga ustalenia koloru osobno dla każdego z widocznych na ekranie pikseli. Dodatkowo, ze względu na zasadę działania, algorytm ten nie jest w stanie obsługiwać światła rozproszonego, symulować dyfrakcji lub rozszczepienia światła.

*na tym można zakończyć, ostatni punkt wydaje się opcjonalny*
## 5. Zaawansowane pojęcia (BRDF i Sampling)
- **BRDF (Dwukierunkowa Funkcja Rozkładu Odbicia):**
    - To matematyczny opis materiału. Funkcja $f(L, V)$, która mówi: "jeśli światło pada z kierunku $L$, to ile procent energii odbije się w kierunku oka $V$?".
    - Dla lustra BRDF to "szpilka" (odbija tylko w jednym kierunku). Dla kartki papieru to półkula (rozprasza wszędzie).
- **Super-sampling / Antyaliasing:**
    - Problem: "Schodki" na krawędziach (aliasing).
    - Rozwiązanie: Zamiast jednego promienia przez środek piksela, wypuszczasz np. 4 lub 16 losowych promieni w obrębie piksela i uśredniasz wynik (Twoje notatki wspominają o "metodzie próbkowania przestrzeni").

## 6. Porównanie Ray Tracing vs Radiosity (Metoda Energetyczna)

| Cecha         | Ray Tracing                               | Radiosity (Metoda Energetyczna)                                                |
| ------------- | ----------------------------------------- | ------------------------------------------------------------------------------ |
| **Metoda**    | Śledzenie promieni (punktowe)             | Podział na płaty (meshing) i bilans energii                                    |
| **Efekty**    | Ostre cienie, lustra, szkło               | Miękkie cienie, Color Bleeding (czerwona podłoga barwi białą ścianę)           |
| **Zależność** | Zależy od pozycji kamery (View-dependent) | Niezależne od kamery (View-independent) - raz policzone działa z każdej strony |
| **Koszt**     | Rośnie z rozdzielczością ekranu           | Rośnie ze stopniem komplikacji geometrii                                       |
