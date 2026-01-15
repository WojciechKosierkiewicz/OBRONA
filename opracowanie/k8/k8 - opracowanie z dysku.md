# Mechanizmy systemu operacyjnego wspomagające synchronizację procesów.

Synchronizacja służy do koordynacji procesów/wątków współdzielących zasoby, aby uniknąć **wyścigu (Race Condition)**. Podstawowe mechanizmy dostarczane przez jądro systemu (kernel) to **Muteksy**, **Semafory** i **Zmienne warunkowe**, a na niższym poziomie **Blokady wirujące (Spinlocki)**.

---

## 0. Wstępne definicje
## Sekcja krytyczna
- ciąg operacji wykonywanych na zasobie, który może być wykonywany przez tylko **jeden** z potencjalnie wielu procesów
- Warunki poprawnego rozwiązania sekcji krytycznej:
    - w sekcji krytycznej działa naraz tylko jeden proces
    - proces nie może się zatrzymać w sekcji krytycznej
    - zatrzymanie procesu w sekcji lokalnej nie może blokować wejścia innym procesom do sekcji krytycznej
    - każdy proces musi koniec końców wejść do sekcji krytycznej

## Synchronizacja procesów
Występuje gdy procesy współbieżne ze sobą współpracują. Najbardziej popularne przypadki kiedy potrzebujemy synchronizacji:
### Procesy współdzielą pewną strukturę danych
Operacje na tej strukturze powinny być zatomizowane, tzn. że tylko jeden z procesów naraz jest w stanie ją modyfikować. Jest to przykład **sekcji krytycznej** w procesie.

### Wyniki działania procesu stanowią dane dla innego procesu
- Kolejny proces jest w stanie dopiero przetwarzać dane po zakończeniu obliczeń przez jego poprzednika.
- Zależność jest określana problemem producenta-konsumenta

### Procesy korzystają z pewnej wspólnej puli zasobów
- pobierają i zwalniają zasoby wedle potrzeb
- przykładem tej synchronizacji jest problem jedzących filozofów
## 1. Podstawowe mechanizmy synchronizacji
## Muteks (Mutex - Mutual Exclusion)
- **Co to jest:** Binarny "zamek" (0/1). Tylko jeden wątek może go posiadać w danej chwili. Jeśli inny wątek chce wejść, a muteks jest zajęty, system operacyjny (OS) **usypia** ten wątek (context switch).
- **Własność:** Muteks ma "właściciela". Tylko wątek, który zamknął muteks, może go otworzyć.
- **Koszt:** Uśpienie wątku jest kosztowne (przełączanie kontekstu zajmuje czas procesora), dlatego muteksy są dobre dla dłuższych sekcji krytycznych (np. zapis do pliku/bazy).
- **Przykład:** `synchronized` w Javie, `sync.Mutex` w Go.​
## **Semafor (Semaphore)**
- **Co to jest:** Licznik całkowity (nieujemny) z dwiema operacjami atomowymi: `P` (Wait/Acquire - zmniejsz) i `V` (Signal/Release - zwiększ).
- **Typy:**
    - _Binarny:_ Działa jak muteks, ale nie ma właściciela (jeden wątek może zablokować, inny odblokować).
    - _Licznikowy:_ Pozwala na dostęp do zasobu $N$ wątkom naraz (np. pula połączeń do bazy danych).
- **Różnica Muteks vs Semafor:** Muteks służy do ochrony danych (wykluczania), semafor służy do sygnalizacji i sterowania przepływem (np. "zadanie skończone, możesz brać wynik").​
- Ponadto wśród semaforów wyróżniamy:
	- semafory ze zbiorem procesów oczekujących - nie określają kolejności wejścia procesów. Istnieje możliwość, ze któryś z procesów nigdy nie wejdzie do sekcji krytycznej.
	- semafory z kolejką procesów oczekujących - procesy czekające są umieszczane w kolejce FIFO
## **Blokada wirująca (Spinlock)**
- **Co to jest:** Wątek nie idzie spać, tylko "kręci się" w pętli `while(isLocked) {}` sprawdzając, czy blokada puściła (busy-waiting).
- **Zalety:** Brak kosztownego przełączania kontekstu. Bardzo szybki dla _bardzo krótkich_ sekcji krytycznych.
- **Wady:** Zjada procesor (100% użycia rdzenia). W systemach jednoprocesorowych bez sensu. Używane głęboko w jądrze Linuxa.​​
## **Monitor**
- **Co to jest:** Wysokopoziomowa konstrukcja (np. w Javie), która łączy w sobie muteks (do ochrony danych) i zmienne warunkowe (do czekania). Metody oznaczone jako `synchronized` to w praktyce wejście do monitora obiektu​

---
## 2. Klasyczne problemy synchronizacji
### Zagłodzenie
- dany proces nie jest w stanie zakończyć zadania gdyż nie ma dostępu do współdzielonego zasobu.
- Przykład problem czytelników i pisarzy
### Deadlock (zakleszczenie)
- conajmniej dwie akcje czekają na siebie nawzajem i żadna z nich nie jest w stanie się zakończyć
- Przykład problem jedzących filozofów

---
## 3. Zmienne Warunkowe (Condition Variables)
Bardzo ważne w Javie (`wait()`, `notify()`) i Go (`sync.Cond`).
- **Mechanizm:** Pozwalają wątkowi "spać" wewnątrz sekcji krytycznej (zwolnić muteks), czekając na spełnienie pewnego warunku (np. "czy bufor nie jest już pusty?").
- **Schemat działania:**
    1. Weź Muteks.
    2. Sprawdź warunek (w pętli `while`!).
    3. Jeśli niespełniony -> `wait()` (OS zdejmuje blokadę i usypia wątek).
    4. Inny wątek zmienia stan i robi `signal()`/`notify()`.
    5. Obudzony wątek ponownie bierze Muteks i sprawdza warunek.

