Współczesne systemy informatyczne w ochronie zdrowia są projektowane w oparciu o **architekturę modułową**. Zamiast jednego, monolitycznego systemu realizującego wszystkie funkcje, stosuje się wyspecjalizowane podsystemy, które komunikują się ze sobą za pomocą standardów wymiany danych, takich jak HL7. Takie podejście zwiększa elastyczność, niezawodność i możliwość rozwoju systemów szpitalnych.

## HIS – Hospital Information System
**HIS** stanowi centralny element infrastruktury informatycznej szpitala i pełni rolę systemu nadrzędnego, który integruje pozostałe podsystemy. Odpowiada on za obsługę procesów administracyjnych oraz ogólnych danych medycznych pacjenta.

Do głównych funkcji HIS należą m.in.:
- obsługa ruchu chorych (ADT – przyjęcia, wypisy i transfery), 
- prowadzenie podstawowej dokumentacji medycznej,
- rozliczenia z płatnikiem, np. NFZ,
- zarządzanie personelem i grafikami dyżurów.

Z perspektywy architektury HIS jest miejscem, w którym lekarz **zleca badania diagnostyczne**, a następnie **odbiera ich wyniki w formie opisowej lub liczbowej**, pochodzące z systemów takich jak RIS, PACS czy LIS.

## RIS i PACS – systemy radiologiczne
**RIS (Radiology Information System)** odpowiada za organizację pracy zakładu radiologii. Zarządza on procesami administracyjnymi i klinicznymi, takimi jak rejestracja pacjentów na badania, planowanie kolejki, obsługa zleceń oraz tworzenie opisów badań przez radiologów. RIS przetwarza głównie dane tekstowe i metadane, a jego komunikacja z innymi systemami opiera się przede wszystkim na standardzie HL7.

**PACS (Picture Archiving and Communication System)** jest systemem przeznaczonym do archiwizacji, dystrybucji i wizualizacji obrazów diagnostycznych. Przechowuje on duże pliki graficzne pochodzące z tomografów, rezonansów czy aparatów RTG, udostępnia je na stacjach opisowych oraz umożliwia ich przeglądanie. Dane w PACS są zapisywane i przesyłane zgodnie ze standardem DICOM.

W praktyce RIS i PACS ściśle ze sobą współpracują: RIS zarządza informacją o pacjencie i zleceniu badania, natomiast PACS przechowuje właściwe obrazy. Radiolog korzysta z RIS do tworzenia opisu, a system automatycznie pobiera odpowiednie obrazy z PACS.

### LIS – Laboratory Information System
**LIS** to system dedykowany obsłudze laboratoriów diagnostycznych, takich jak laboratoria analiz krwi czy moczu. Jego specyfika polega na konieczności bezpośredniej integracji z analizatorami laboratoryjnymi.

LIS odpowiada za:
- obsługę próbek i kodów kreskowych,
- automatyczny odczyt wyników z urządzeń pomiarowych,
- kontrolę jakości badań i kalibrację aparatury,
- przekazywanie ustrukturyzowanych wyników liczbowych do systemu HIS.

W odróżnieniu od PACS, LIS nie operuje na obrazach, lecz na danych liczbowych i tekstowych, które są kluczowe dla dalszej diagnostyki.

### Zalety i wady modułowości
Do głównych zalet architektury modułowej należą:
- wysoka specjalizacja systemów, lepiej dopasowanych do potrzeb poszczególnych oddziałów,
- większa niezawodność – awaria jednego systemu nie paraliżuje całego szpitala,
- możliwość niezależnej modernizacji poszczególnych komponentów, np. wymiany PACS bez ingerencji w HIS.

Jednocześnie takie podejście wiąże się z istotnymi wyzwaniami. Największym z nich jest **integracja i interoperacyjność** systemów pochodzących od różnych producentów, co wymaga stałego utrzymywania interfejsów komunikacyjnych. Dodatkowym problemem jest redundancja danych pacjenta w wielu bazach oraz wyższe koszty licencyjne i serwisowe. Z perspektywy użytkownika klinicznego wyzwaniem bywa również fragmentacja interfejsu, dlatego dąży się do integracji systemów poprzez mechanizmy takie jak Single Sign-On.

### Standardy wymiany informacji
Funkcjonowanie całego ekosystemu systemów medycznych byłoby niemożliwe bez ustandaryzowanych mechanizmów komunikacji.

**HL7** jest standardem wykorzystywanym do wymiany danych tekstowych i komunikatów medycznych, takich jak zlecenia badań czy wyniki laboratoryjne.  
**DICOM** służy do przesyłania i przechowywania obrazów diagnostycznych wraz z metadanymi pacjenta i badania.