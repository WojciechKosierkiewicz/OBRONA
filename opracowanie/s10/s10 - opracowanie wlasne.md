REST (_Representational State Transfer_) to styl architektoniczny projektowania usług sieciowych, w którym HTTP jest wykorzystywane jako standardowy protokół komunikacji. Kluczowa idea REST polega na tym, że aplikacja nie „wywołuje funkcji na serwerze”, tylko **operuje na zasobach** (np. pacjentach, wizytach, wynikach badań), a działania na tych zasobach realizuje się przy użyciu semantyki metod HTTP.

### Zasoby identyfikowane przez URI
W REST podstawowym pojęciem jest **zasób** (_resource_) – czyli logiczny byt w systemie, np. `Patient`, `Appointment`, `LabResult`. Każdy zasób (lub kolekcja zasobów) jest jednoznacznie identyfikowany przez **URI** (często URL w praktyce), np.:
- `/patients` – kolekcja pacjentów
- `/patients/123` – konkretny pacjent o id=123
- `/patients/123/visits` – wizyty danego pacjenta

URI opisuje **co** adresujemy (zasób), a nie **jaką operację** wykonujemy. Dlatego w REST unikamy endpointów typu `/getPatient` czy `/deleteVisit`, bo operacje wynikają z metody HTTP.

### Klasyczne metody HTTP w REST
Najczęściej używane metody w REST to:
- **GET** – pobranie reprezentacji zasobu (np. dane pacjenta). Nie powinno zmieniać stanu zasobu po stronie serwera.
- **POST** – utworzenie nowego zasobu w kolekcji (np. dodanie pacjenta w `/patients`) albo uruchomienie procesu, gdy nie pasuje to do CRUD.
- **PUT** – pełna aktualizacja zasobu (nadpisanie reprezentacji), np. `/patients/123`.
- **PATCH** – częściowa aktualizacja zasobu (zmiana wybranych pól).
- **DELETE** – usunięcie zasobu.

W praktyce warto znać też własności metod:
- **bezpieczne** (safe): typowo **GET** (nie zmienia danych),
- **idempotentne**: **GET, PUT, DELETE** (wielokrotne wykonanie daje ten sam efekt końcowy), a **POST** zwykle nie jest idempotentny.

REST mocno wykorzystuje też standardowe kody odpowiedzi HTTP, np. `200 OK`, `201 Created`, `204 No Content`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`.

### Bezstanowość (statelessness)

Jedną z fundamentalnych zasad REST jest **bezstanowość**: każde żądanie HTTP powinno zawierać wszystkie informacje potrzebne do jego obsługi. Serwer nie powinien polegać na „pamiętaniu” stanu sesji klienta między żądaniami.

Co to oznacza w praktyce?
- Autoryzacja jest przesyłana przy każdym żądaniu (np. token JWT w nagłówku `Authorization`).
- Serwer nie trzyma „kontekstu użytkownika” w pamięci aplikacji – dzięki temu łatwiej skalować system horyzontalnie (wiele instancji serwera).
- Stan aplikacji (np. „koszyk”, „formularz wypełniany krokami”) powinien być po stronie klienta albo w zewnętrznym magazynie (np. baza/Redis), a nie w pamięci pojedynczej instancji.

**Zalety bezstanowości:** skalowalność, prostsza infrastruktura, mniejsze ryzyko problemów z sesjami.  
**Wady:** czasem większy narzut (więcej danych w każdym żądaniu) i konieczność świadomego projektowania procesów wieloetapowych.

### Format reprezentacji danych: JSON i XML

REST mówi o „reprezentacji zasobu” – czyli jak dane zasobu są przesyłane. Najczęściej spotkasz:
#### JSON
**Kiedy stosować:** API webowe i mobilne, usługi mikroserwisowe, nowoczesne integracje.  
**Zalety:** lekki, czytelny, naturalnie mapuje się na obiekty w językach programowania, bardzo dobrze wspierany w JS i narzędziach webowych.  
**Wady:** słabsze wsparcie dla formalnego walidowania struktury (choć istnieje JSON Schema), brak „wbudowanej” semantyki dokumentów i mniej rozbudowane mechanizmy nazw przestrzeni niż w XML.
#### XML
**Kiedy stosować:** systemy enterprise, dokumenty medyczne i integracje oparte o standardy dokumentowe (np. HL7 CDA), środowiska, gdzie liczy się ścisła walidacja i kontrakty.  
**Zalety:** bardzo dojrzałe mechanizmy walidacji (XSD), dobre dla dokumentów hierarchicznych, wsparcie dla przestrzeni nazw, podpisu i szyfrowania w standardach dokumentowych.  
**Wady:** większy narzut (więcej znaków), bywa mniej wygodny w codziennym API, trudniejszy w ręcznym odczycie i parsowaniu niż JSON.

W HTTP wybór formatu jest zwykle negocjowany przez nagłówki:
- `Content-Type` – format danych wysyłanych do serwera,
- `Accept` – format oczekiwany w odpowiedzi.