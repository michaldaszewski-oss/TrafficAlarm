# 📋 Dziennik Zmian (Changelog) - TrafficAlarm

Wszystkie wydania aplikacji **TrafficAlarm** wraz z podsumowaniem wprowadzonych funkcji i poprawek.

---

## [v1.9.3] - 2026-11-15
### 🗺️ Inteligentny geocoding & Obsługa tras międzymiastowych
- **Wyeliminowanie błędu wyznaczania tras międzymiastowych (np. Warszawa - Kraków):**
  - Rozwiązano problem, w którym podanie trasy między miastami (np. Warszawa ➔ Kraków) zwracało błędną odległość `153 km` i czas `114 min` (wynikający z dopasowania ul. Krakowskiej w Skarżysku-Kamiennej zamiast właściwego miasta Kraków).
- **Wielokryterialny ranking wyników geocodingu (Photon / Nominatim):**
  - Przy zapytaniach o miasto wprowadzono priorytetyzację encji miejskich i administracyjnych (`place=city`, `place=town`, `boundary=administrative`).
  - Wprowadzono karę punktową dla ulic noszących nazwę poszukiwanego miasta (np. ulica Krakowska w innej miejscowości nie przesłoni już właściwego Krakowa).
  - Wyszukiwanie z domyślnym filtrem krajowym (`countrycode=pl`) oraz automatycznym fallbackiem globalnym dla zapytań zagranicznych.
- **Skalowanie szczytu komunikacyjnego dla tras długodystansowych:**
  - Zatory i godziny szczytu naliczane są realistycznie dla wyjazdu i wjazdu do aglomeracji miejskich (odcinki miejskie), eliminując sztuczne zawyżanie czasu na drogach ekspresowych i autostradach (S7/A1/A4).
- **Dynamiczne okno czuwania dostosowane do wielogodzinnych podróży:**
  - Zwiększono maksymalne okno czuwania z 3h do 12h, gwarantując, że rozpoczęcie monitorowania zawsze wyprzedza czas wyjazdu i dzwonienia alarmu przygotowawczego.
- **Ujednoznacznienie zapytań do Google Routes API i Distance Matrix:**
  - Automatyczny kontekst krajowy zapobiegający błędnemu rozpoznawaniu polskich miast przez globalne silniki Google.
- **Awaryjne szacowanie odległości Haversine:**
  - W przypadku chwilowej niedostępności serwerów OSRM aplikacja szacuje dystans i czas na podstawie wyznaczonych współrzędnych i współczynnika krętości dróg (zamiast sztywnego 12.4 km).

---

## [v1.9.2] - 2026-11-15
### 🚗 Realistyczna predykcja trasy & Izolacja historii per alarm
- **Wykres predykcji bez zmyślonych danych wstecz:**
  - Oś czasu wykresu rozpoczyna się ściśle od **TERAZ (`t = 0`)** do przodu (do momentu wyjazdu i dotarcia do celu).
  - Usunięto sztuczne generowanie punktów w przeszłości (poprzednio od `-45 min`), które tworzyło fikcyjne dane przed uruchomieniem monitorowania.
- **Wyeliminowanie fałszywego skoku korka (z 25 do 40+ min):**
  - Usunięto automatyczne zawyżanie czasu dojazdu o 60-70% w środku dnia i w weekendy.
  - Model prognozy opiera się na **rzeczywistych danych**: jeśli ruch na trasie jest stabilny, prognoza pozostaje płaska i wierna pomiarom (np. 25 min). Wzrost jest prognozowany tylko przy realnie wykrytym narastaniu lub w godzinach rzeczywistego szczytu roboczego (pon.–pt.).
- **Ścisła izolacja historii pomiarów dla każdego alarmu:**
  - Każdy profil alarmu gromadzi wyłącznie własne odczyty powiązane z trasą (`alarmId`).
  - Wyeliminowano mieszanie czasów dojazdów pomiędzy różnymi alarmami i trasami.
- **Zarządzanie historią trasy:**
  - Dodano przycisk *„Wyczyść historię”* bezpośrednio w zakładce pomiarów aktywnej trasy.
  - Wprowadzono automatyczne resetowanie historii trasy przy edycji adresu startowego lub docelowego w danym alarmie.

---

## [v1.9.1] - 2026-11-15
### 🎯 Poprawka godziny celu na ekranie alarmu & Hotfix OTA
- **Poprawka planowanej godziny u celu na ekranie budzika:**
  - Usunięto błąd, przez który na pełnoekranowym ekranie dzwoniącego alarmu godzina u celu była na sztywno zafiksowana na domyślnej wartości `08:00` (zamiast rzeczywistej godziny dzwoniącego alarmu, np. `14:00`).
  - Precyzyjne przekazywanie `targetHour` i `targetMinute` przez cały łańcuch wywołań (`AlarmScheduler` ➔ `TrafficAlarmReceiver` ➔ `AlarmService` ➔ `AlarmActivity`).
- **Eliminacja wielokrotnego dzwonienia przy sprawdzaniu sytuacji:**
  - Dodano dedykowaną flagę `hasPreparationFiredToday`, debouncing 12s w `AlarmService` oraz pełną izolację alarmów testowych (`test_`).
- **Podbicie wersji do v1.9.1 (kod wersji 11):**
  - Umożliwia natychmiastowe wykrycie i zainstalowanie poprawki przez moduł automatycznych aktualizacji (OTA) w telefonie.

---

## [v1.9] - 2026-11-15
### 📋 Historia zmian (Changelog) w aplikacji
- **Dedykowany przycisk w zakładce Opcje:**
  - Dodano przycisk *„Historia zmian (Changelog)”* w sekcji aktualizacji oraz w sekcji informacyjnej aplikacji.
  - Otwiera pełne okno dialogowe z kompletnym, interaktywnym wykazem wszystkich wydań aplikacji (od v1.0 do v1.9.1).
- **Interaktywne okno historii wersji:**
  - Wersje oznaczone czytelnymi etykietami (np. *Aktualna v1.9.1*, *Widżet v1.8*, *Predykcja ruchu v1.7*, *Adaptacyjny interwał v1.6*, itd.).
  - Szczegółowe podsumowanie nowości, usprawnień i poprawek działania budzika dla każdego wydania.
  - Bezpośredni przycisk przejścia do repozytorium GitHub i oficjalnego dokumentu CHANGELOG.md.

---

## [v1.8] - 2026-11-15
### 📱 Nowości i poprawki
- **Pełna modernizacja widżetu ekranu głównego:**
  - Widżet dynamicznie pobiera i śledzi **najbliższy chronologicznie aktywny alarm** z listy użytkownika (poprzednio korzystał ze starych danych pojedynczego profilu).
  - Wyświetlanie aktualnej nazwy trasy, punktu startu i celu (`📍 Start ➔ Cel`).
  - Dokładny stan: czas do wyjścia, planowana godzina wyjścia, czas dojazdu w korkach, odległość w kilometrach oraz status spóźnienia.
  - Przycisk odświeżenia na widżecie odpytuje o ruch bezpośrednio dla aktywnej trasy.
- **Aktualizacja interfejsu aplikacji:**
  - Zmiana podpisu przycisku z *„Dotarłem / Przełącz na kolejny dzień”* na **„Już wyszedłem / Przełącz na kolejny dzień”**.

---

## [v1.7] - 2026-11-15
### 🚗 Ruch i estymacje dla przyszłych dni
- **Realistyczne szacowanie czasu na przyszłe terminy (koniec z 19 min w poniedziałkowy poranek):**
  - Do Google Routes API przekazywany jest parametr `departureTime` (czas UTC w formacie RFC3339) oraz `routingPreference: TRAFFIC_AWARE_OPTIMAL` – uwzględnia predykcję ruchu Google na dany dzień tygodnia i godzinę.
  - W darmowym routingu OpenStreetMap/OSRM wprowadzono analizę dnia tygodnia: w porannym szczycie roboczym (6:30–9:30) stosowany jest mnożnik korkowy `1.55x`.
  - **Zabezpieczenie na bazie historii (`previousDurationMin`):** aplikacja nie pozwoli zaniżyć czasu dojazdu poniżej dotychczasowych rzeczywistych przejazdów w szczycie na danej trasie.
### 🔔 Dzwonienie i system dwóch alarmów
- **Limit dzwonienia do 10 sekund:** dźwięk i wibracja wyłączają się samoczynnie po dokładnie 10 sekundach (*„to nie jest budzik a jedynie alarm”*).
- **System dokładnie 2 alarmów:**
  - **Alarm 1 (Przygotowanie):** dzwoni z wyprzedzeniem przed wyjściem (np. 15 min wcześniej). Ekran: *„CZAS SIĘ ZBIERAĆ!”*.
  - **Alarm 2 (Czas wyjścia):** dzwoni dokładnie o wyliczonej godzinie wyjścia. Ekran: *„CZAS WYJŚĆ Z DOMU!”*.
- **Drzemka (+5 min):** odkłada bieżący alarm o 5 minut i dzwoni ponownie przez 10 sekund.
### 🔄 Aktualizacje
- Wydłużenie limitu czasu pobierania (read timeout do 120s), omijanie pamięci podręcznej CDN (`?nocache=...`) oraz przycisk *„Pobierz bezpośrednio przez przeglądarkę”* w oknie aktualizacji.

---

## [v1.6] - 2026-11-15
### ⏱️ Adaptacyjny interwał i eliminacja sztywnego okna
- **Automatyczne zacieśnianie odpytywania o ruch:**
  - do **60 sekund** tuż przed wyjściem ($\le 15$ min) lub przy gwałtownym skoku korka ($\ge 2$ min),
  - do **120 sekund** przy 16–30 min do wyjścia,
  - do **180 sekund** przy 31–60 min do wyjścia.
- **Eliminacja sztywnego 90-minutowego okna monitorowania:** okno czuwania liczone elastycznie i proporcjonalnie do długości trasy (np. ~50-60 min dla 30 min jazdy zamiast stałych 90 min).
- Rozdzielenie częstotliwości sprawdzania korków od momentu rozpoczęcia czuwania.

---

## [v1.5] - 2026-11-15
### 🧭 Chronologia alarmów i podgląd trasy
- **Dynamiczne sortowanie listy alarmów:**
  - Najbliższy chronologicznie włączony alarm automatycznie trafia na samą górę listy z wyróżnieniem `⭐ NAJBLIŻSZY`.
  - Spóźniony dojazd otrzymuje najwyższy priorytet na górze ekranu.
- **Podgląd trasy na żywo w oknie edycji/tworzenia alarmu:**
  - Natychmiastowa kalkulacja dystansu, czasu przejazdu oraz podgląd godziny wyjścia już w trakcie wpisywania adresu.

---

## [v1.4] - 2026-11-15
### 📊 Interaktywna historia dynamiki korka
- **W pełni interaktywny wykres korków:**
  - Obsługa gestu przeciągania palcem po wykresie z dynamicznym pionowym kursorem.
  - Pływający dymek (tooltip) pokazujący dokładną godzinę, przewidywany czas przejazdu i natężenie ruchu.
  - Suwak czasu umożliwiający weryfikację czasu dojazdu dla dowolnej minuty poranka.
  - Symulacja przyjazdu do celu: sprawdzenie, o której godzinie dotrze się na miejsce wyjeżdżając o wybranej porze.

---

## [v1.3] - 2026-11-15
### 🚗 Tryb spóźnienia i historia pomiarów
- **Tryb spóźnienia (Late Commute Tracking):**
  - Aplikacja po minięciu godziny wyjścia nie przeskakuje na kolejny dzień, lecz na żywo oblicza spóźnienie i estymowaną godzinę dotarcia do pracy przy wyjeździe w danej chwili.
- Wprowadzenie historii próbek ruchu i bazy danych trendów.

---

## [v1.2] - 2026-11-15
### 📱 Stabilność na smartfonach Samsung (One UI)
- Zastosowanie natywnego mechanizmu `AlarmManager.setAlarmClock`:
  - Gwarancja wybudzenia telefonu z głębokiego trybu uśpienia (Doze / Samsung App Power Management).
  - Ikona budzika na pasku stanu One UI.
- Dodanie dedykowanej karty z instrukcją konfiguracji uprawnień baterii i autostartu dla urządzeń Samsung.

---

## [v1.1] - 2026-11-15
### ⚙️ Poprawki instalatora i autoupdate
- Usprawnienie modułu automatycznych aktualizacji z GitHuba (`AppUpdateManager`).
- Dodanie sum kontrolnych i weryfikacji integralności pobieranego pliku APK.
- Zunifikowanie oficjalnej nazwy pliku instalacyjnego na `TrafficAlarm-app.apk`.

---

## [v1.0] - 2026-11-15
### 🚀 Pierwsze wydanie TrafficAlarm
- Automatyczne monitorowanie ruchu i korków na trasie dom-praca.
- Obsługa oficjalnego Google Routes API oraz bezpłatnego silnika OpenStreetMap / OSRM.
- Inteligentny budzik drogowy dynamicznie dostosowujący czas dzwonienia do sytuacji na drodze.
- Ciemny motyw Material 3 w kolorystyce Dark Slate (`#0F172A`).
- Podstawowy widżet na ekran główny Androida.
