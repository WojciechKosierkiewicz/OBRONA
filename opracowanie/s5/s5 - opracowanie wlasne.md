### **1. Funkcja jasności**

Funkcja jasności $f(x,y)$ opisuje **rozkład natężenia światła w obrazie ciągłym**, gdzie każdemu punktowi przestrzeni przypisana jest wartość jasności. W obrazie w skali szarości funkcja ta przyjmuje wartości rzeczywiste proporcjonalne do ilości światła docierającego do sensora, a w przypadku obrazów kolorowych występuje osobno dla każdego kanału barwnego.

### **2. Próbkowanie**

Próbkowanie polega na **zamianie obrazu ciągłego w dziedzinie przestrzennej na postać dyskretną**, poprzez pobieranie wartości funkcji jasności w regularnie rozmieszczonych punktach siatki. Gęstość próbkowania decyduje o tym, jak dokładnie odwzorowane zostaną szczegóły obrazu – zbyt rzadkie próbkowanie prowadzi do utraty informacji i powstawania aliasingu.

### **3. Kwantyzacja**

Kwantyzacja polega na **zamianie ciągłego zakresu wartości jasności na skończony zbiór poziomów**, co oznacza, że każda zmierzona wartość jest zaokrąglana do najbliższego dopuszczalnego poziomu. Liczba poziomów kwantyzacji wpływa bezpośrednio na jakość odwzorowania przejść tonalnych – zbyt mała liczba poziomów powoduje widoczne skoki jasności, czyli posteryzację.

### **4. Rozdzielczość przestrzenna**

Rozdzielczość przestrzenna określa **zdolność obrazu do odwzorowania drobnych detali**, zależną od liczby próbek w poziomie i pionie lub liczby pikseli na jednostkę długości. Im wyższa rozdzielczość przestrzenna, tym więcej szczegółów może zostać zarejestrowanych, pod warunkiem odpowiedniego próbkowania sygnału.

### **5. Rozdzielczość tonalna**

Rozdzielczość tonalna określa **liczbę możliwych poziomów jasności**, jakie mogą być przypisane pojedynczemu pikselowi, i jest bezpośrednio związana z liczbą bitów użytych do zapisu obrazu. Wyższa rozdzielczość tonalna umożliwia płynniejsze przejścia jasności i dokładniejsze odwzorowanie kontrastu.