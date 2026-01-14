Głównym celem inżynierii danych medycznych jest zapewnienie **interoperacyjności**, czyli zdolności różnych systemów informatycznych do wymiany danych w taki sposób, aby były one nie tylko poprawnie przesyłane, ale również jednoznacznie interpretowane. Aby to osiągnąć, konieczne jest ustandaryzowanie zarówno **struktury danych** (rekordów pacjenta, formatów wymiany), jak i ich **znaczenia semantycznego** (klasyfikacji, słowników i ontologii medycznych).

## Systemy rekordów pacjenta (EMR, EHR, PHR)

W informatyce medycznej rozróżnia się trzy główne typy systemów gromadzenia danych o pacjencie - **EMR, EHR I PHR**

1. **EMR (Electronic Medical Record)** to elektroniczny odpowiednik papierowej dokumentacji medycznej prowadzonej w ramach jednej placówki. System EMR służy głównie do obsługi procesu diagnozy i leczenia w obrębie jednej organizacji. Dane zgromadzone w EMR zazwyczaj nie są automatycznie udostępniane innym podmiotom, a ich wymiana, jeśli w ogóle występuje, ma często postać wydruków lub plików PDF.

2. **EHR (Electronic Health Record)** stanowi szerszą koncepcję rekordu zdrowotnego, który „podąża za pacjentem” pomiędzy różnymi placówkami ochrony zdrowia. EHR zakłada istnienie interoperacyjnych systemów, zdolnych do automatycznej wymiany danych pomiędzy szpitalami, lekarzami POZ, laboratoriami czy aptekami. W Polsce elementem ekosystemu EHR jest m.in. Internetowe Konto Pacjenta.

3. **PHR (Personal Health Record)** to rekord zdrowotny zarządzany bezpośrednio przez pacjenta. Może on zawierać dane pochodzące z urządzeń typu wearable, aplikacji mobilnych lub własne notatki pacjenta. PHR nie zastępuje dokumentacji medycznej, ale może ją uzupełniać.

## Klasyfikacje medyczne (semantyka danych)

Aby systemy informatyczne mogły poprawnie interpretować dane kliniczne, niezbędne jest stosowanie **ustrukturyzowanych kodów**, a nie wyłącznie opisów w języku naturalnym. W tym celu wykorzystuje się klasyfikacje i terminologie medyczne. Przykładami takich są m.in. **ICD 10/11 oraz SNOMED CT**

**ICD-10 oraz ICD-11** (International Classification of Diseases) są międzynarodowymi klasyfikacjami chorób stosowanymi przede wszystkim do celów statystycznych, epidemiologicznych oraz rozliczeniowych (przykładowo z NFZ). Klasyfikacja ICD ma strukturę hierarchiczną, przypominającą drzewo, gdzie kolejne poziomy coraz dokładniej precyzują rozpoznanie. Wersja ICD-11 została zaprojektowana jako w pełni cyfrowa, z myślą o integracji z systemami IT, i umożliwia tzw. polikodowanie, czyli łączenie kilku kodów w jeden opis kliniczny. Mimo to w Polsce w praktyce klinicznej nadal dominuje ICD-10.

**SNOMED CT** to znacznie bardziej zaawansowana terminologia kliniczna, której celem jest precyzyjny opis stanu pacjenta, wspomaganie decyzji klinicznych oraz analiza danych medycznych. W przeciwieństwie do ICD nie jest to prosta lista kodów ułożona w drzewo, lecz rozbudowany system pojęć powiązanych ze sobą siecią relacji, co pozwala dokładniej opisywać stan kliniczny pacjenta. Pozwala modelować dane kliniczne w sposób znacznie bogatszy semantycznie, co ma kluczowe znaczenie w systemach analitycznych, Big Data oraz w nowoczesnych systemach EHR.

## Standardy wymiany danych – transport i składnia

Po zakodowaniu danych medycznych konieczne jest ich bezpieczne i jednoznaczne przesyłanie pomiędzy systemami. W tym obszarze kluczową rolę odgrywają standardy opracowane przez organizację HL7.

**HL7 w wersji 2** jest najstarszym, ale wciąż bardzo powszechnym standardem komunikacji wewnątrz systemów szpitalnych. Opiera się na prostych komunikatach tekstowych, w których pola oddzielane są znakami specjalnymi. Standard ten jest szybki i skuteczny, ale mało elastyczny i trudny w walidacji, co stanowi jego główne ograniczenie.

**HL7 CDA (Clinical Document Architecture)** to standard oparty na języku XML, przeznaczony do wymiany pełnych dokumentów medycznych, takich jak wypisy ze szpitala czy wyniki badań. W Polsce CDA, w ramach Polskiej Implementacji Krajowej, stanowi podstawę prawnej definicji Elektronicznej Dokumentacji Medycznej.

**HL7 FHIR (Fast Healthcare Interoperability Resources)** jest nowoczesnym standardem wymiany danych, zaprojektowanym z myślą o technologiach webowych. Wykorzystuje architekturę REST API oraz formaty JSON lub XML i opiera się na pojęciu zasobów, takich jak Patient, Observation czy Medication.

## Elektroniczna Dokumentacja Medyczna w Polsce

W polskim systemie prawnym **EDM** jest pojęciem ściśle zdefiniowanym. Nie każda dokumentacja prowadzona w formie elektronicznej spełnia kryteria EDM. Aby dokument został uznany za EDM, musi być wytworzony zgodnie ze standardem HL7 CDA, podpisany jednym z dopuszczalnych podpisów elektronicznych oraz zarejestrowany w centralnym systemie P1. Dzięki temu dokumentacja może być udostępniona innym uprawnionym podmiotom ochrony zdrowia.