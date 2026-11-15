# 📋 Dziennik Zmian (Changelog) - TrafficAlarm

Wszystkie wydania aplikacji **TrafficAlarm** wraz z podsumowaniem wprowadzonych funkcji i poprawek.

---

## [v1.9] - 2026-11-15
### 📋 Historia zmian (Changelog) w aplikacji
- **Dedykowany przycisk w zakładce Opcje:**
  - Dodano przycisk *„Historia zmian (Changelog)”* w sekcji aktualizacji oraz w sekcji informacyjnej aplikacji.
  - Otwiera pełne okno dialogowe z kompletnym, interaktywnym wykazem wszystkich wydań aplikacji (od v1.0 do v1.9).
- **Interaktywne okno historii wersji:**
  - Wersje oznaczone czytelnymi etykietami (np. *Aktualna v1.9*, *Widżet v1.8*, *Predykcja ruchu v1.7*, *Adaptacyjny interwał v1.6*, itd.).
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
