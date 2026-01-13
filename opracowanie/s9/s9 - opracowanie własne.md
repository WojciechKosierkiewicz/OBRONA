# RODO
Podstawą prawną przetwarzania danych medycznych w systemach informatycznych jest RODO (Ogólne Rozporządzenie o Ochronie Danych), w kontekście systemu informatycznego kluczowe jest rozróżnienie typów danych:
- **Dane osobowe** - informacje pozwalające zidentyfikować osobę, zarówno bezpośrednio jak i pośrednio (np. PESEL, imię i naziwsko, czy też numer identyfikacyjny pacjenta w bazie czy numer karty pacjenta, jeżeli pozwalają powiązać dane z konkretną osobą)
- **Dane wrażliwe (Dane szczególnej kategorii)** - dane dotyczące zdrowia, genetyczne czy biometryczne, co do zasady przetwarzanie tych danych jest zabronione, chyba że zachodzi jedna z przesłanek zdefiniowanych w ustawie (np. diagnoza medyczna, leczenie, zarządzanie systemami opieki zdrowotnej). Systemy przechowujące takie dane muszą spełniać znacznie wyższe wymagania dotyczące bezpieczeństwa, niż systemy przetwarzające tylko dane osobowe

## Bezpieczeństwo danych
### Ważne pojęcia
Kluczowym aspektem bezpieczeństwa danych są pojęcia anonimizacji i pseudonimizacji.
- **Pseudonimizacja** - proces przetworzenia danych w taki sposób, by nie można było ich przypisać konkretnej osobie, bez użycia dodatkowych informacji. W praktyce oznacza to zastąpienie danych identyfikujących pacjenta losowym identyfikatorem, przy czym klucz umożliwiający ponowną identyfikację jest przechowywany oddzielnie i odpowiednio zabezpieczony. Z prawnego punktu widzenia dane pseudonimizowane nadal są danymi osobowymi, a więc w pełni podlegają przepisom RODO. Takie rozwiązania stosowane są przykładowo wewnątrz szpitala, czy przy przesyłaniu danych między modułami systemu
- **Anonimizacja** - Proces nieodwracalnego usunięcia powiązań między danymi a osobą. Osiąga się to poprzez usunięcie identyfikatorów, generalizację danych, ich agregację lub celowe zaszumienie. Po prawidłowo przeprowadzonej anonimizacji dane przestają być danymi osobowymi, a więc RODO przestaje mieć do nich zastosowanie. Takie dane są wykorzystywane w analizach statystycznych, badaniach naukowych, projektach Big Data czy w trenowaniu modeli uczenia maszynowego.

Systemy wspierające leczenie konkretnego pacjenta wymagają pseudonimizacji danych i ich silnego szyfrowania, w przypadku systemów analitycznych należy dążyć do pełnej anonimizacji danych.

### Techniczny aspekt bezpieczeństwa

System powinien realizować tzw. *triadę CIA*
- Poufność (confidentiality) - Dostęp do danych medycznych mają wyłącznie osoby uprawnione, co w systemach IT realizuje się poprzez szyfrowanie danych w bazie oraz w trakcie transmisji, a także stosowanie silnych mechanizmów uwierzytelniania
- Integralność (integrity) - Polega na zapewnieniu, że dane nie zostaną nieuprawnienie zmodyfikowane, co osiąga się poprzez mechanizmy kontroli spójności, wersjonowanie rekordów oraz regularne kopie zapasowe
- Dostępność (availability) - Dane są dostępne wtedy, gdy są potrzebne, na przykład w sytuacjach nagłych, co wymaga redundancji systemów oraz planów odtwarzania po awarii
Oprócz tych trzech, RODO wprowadza również zasadę rozliczalności (accountability): System musi rejestrować, kto, kiedy i po co oglądał dane pacjenta (logi systemowe). Jest to wymóg prawny - pacjent ma prawo zapytać, kto zaglądał w jego kartę.

## Zgoda pacjenta

Przetwarzanie danych wymaga podstawy prawnej, rozumianej jako zgoda pacjenta. W przypadku leczenia zgoda jest często dorozumiana lub wynika bezpośrednio z przepisów prawa, jako że podmioty lecznicze mają obowiązek prowadzenia dokumentacji medycznej. 
Inaczej wygląda sytuacja w przypadku badań naukowych, projektów wykorzystujących sztuczną inteligencję czy działań marketingowych, gdzie wymagana jest wyraźna, świadoma i dobrowolna zgoda pacjenta. System informatyczny musi umożliwiać rejestrowanie faktu udzielenia zgody, jej zakresu oraz momentu jej cofnięcia, co jest bezpośrednio związane z realizacją prawa do bycia zapomnianym

## Komisje bioetyczne

W przypadku projektów o charakterze eksperymentalnym (np. testy nowego algorytmu diagnostycznego na danych pacjentów, co może wpłynąć na ich diagnozę, lub zbieranie nowych danych w sposób inwazyjny):
- Należy uzyskać zgodę niezależnej Komisji Bioetycznej.
- Komisja sprawdza, czy korzyść z badania przewyższa ryzyko dla pacjenta i czy dane są bezpieczne.
- Dla "zwykłej" aplikacji do zarządzania przychodnią (system klasy HIS/EHR) zgoda komisji zazwyczaj nie jest potrzebna, bo to standardowa procedura administracyjna.