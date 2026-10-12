# Historia zmian — IF-startpage

Nowe wpisy dopisujemy na górze.

---

## v0.1.6 — 2026-10-12

Wygląd startowy i naprawiony edytor wyglądu Zaplecza.

- **Naprawione:** dotknięcie dowolnej kontrolki w edytorze „Wygląd Zaplecza” zmieniało wygląd całego Zaplecza (np. panele robiły się białe). Formularz czyta teraz aktualne wartości z CSS, a zapisuje tylko to, co zmieniono.
- **Naprawione:** ustawienia przycisków (kolory, najechanie, ramka, poświata) działały tylko na podglądzie — teraz zmieniają prawdziwe przyciski Zaplecza.
- **Naprawione:** „Przywróć domyślne” nie cofało zmian do czasu przeładowania strony; kolor ramki przycisków był zawsze czarny; nie dało się ustawić wartości 0 (np. bez zaokrąglenia, bez poświaty).
- **Naprawione:** przy każdym zapisie wygląd z wgraną tapetą zapisywał cały obraz w pliku ustawień.
- **Naprawione:** wgrane logo i tapety znikały po aktualizacji obrazu kontenera — teraz trafiają do wolumenu z konfiguracją (`config/uploads`).
- Nowy przycisk **„Cofnij zmiany”** w edytorze wyglądu Zaplecza.
- **Wygląd startowy Zaplecza:** ciemne panele, spokojne przyciski menu (aktywny bez jaskrawego wypełnienia), wyraźny przycisk główny, czytelny przycisk ostrzegawczy, bez pustego pasa pod tytułem.
- **Wygląd startowy strony:** lżejszy pasek boczny, wyraźny przycisk główny, podpowiedź na pustej stronie, jak dodać pierwsze kafelki.
- Zakładka Helper na świeżej instalacji podpowiada nazwę serwera, adres API i host zamiast pustych pól.
- Serwer sprawdza ustawienia wyglądu Zaplecza przed zapisem (tylko znane pola, bez znaków spoza wartości CSS).

---

## v0.1.5 — 2026-10-12

Porządki i bezpieczeństwo po audycie.

- **PIN wymagany** do edycji kafelków i sekcji, logów kontenerów i sprawdzania portów (wcześniej działały bez PIN-u).
- **Blokada zgadywania PIN-u:** po 5 błędnych próbach blokada na minutę, kolejne serie coraz dłużej (maks. 15 min). Porównanie PIN-u odporne na pomiar czasu.
- **Bez otwartego CORS:** obce strony otwarte w przeglądarce nie mogą wywoływać API serwera.
- Domyślny PIN ujednolicony z przykładem w `compose.yaml` (`1234`); nowy PIN musi mieć min. 4 znaki.
- Aktualizacje: walidacja nazwy stosu przed zapisem `compose.yaml`; wykluczenia i wyjątki nazw konfigurowane zmiennymi `DOCKER_VERSION_EXCLUDE` i `DOCKER_STACK_OVERRIDES` zamiast wpisanych na sztywno.
- Zakładka pakietów apt uczciwie informuje, że kontener nie widzi pakietów serwera, i odsyła do Cockpit.
- Usunięte martwe pliki (`mods/alerts/alerts.js`, `alerts.css`) i odwołania do nieistniejącego `storm-alert` w Kiosku.
- Nazwy „MyHome” i „Homelab NAS” w interfejsie zastąpione nazwą IF-startpage.
- `compose.yaml` na pełnych ścieżkach z `{user}`, zgodnie z README; przypięta wersja obrazu `docker:27-cli`.
- Dodany `THIRD-PARTY.md` (ikony z projektu dashboard-icons) i sekcja Podziękowania w README.

---

## v0.1.4 — 2026-09-18

Pierwsza publiczna wersja IF-startpage: samodzielna strona startowa serwera domowego, działająca w kontenerze Docker.

- **Strona Główna:** kafelki linków w sekcjach, motyw dzień/noc, chowany pasek boczny, widżet CPU/RAM/temperatury w nagłówku.
- **Zaplecze (chronione PIN-em):** Centrum Alertów, Diagnostyka Serwera (Top), Statystyki Live, Aktualizacje kontenerów, Helper & System, edytor wyglądu.
- **Kiosk statusu serwera:** Status serwera, Live (zużycie per kontener), Status kontenerów, Radio.
- **Obraz kontenera** budowany automatycznie przez GitHub Actions i publikowany w `ghcr.io/if-faber/if-startpage`.
- Konfiguracja użytkownika (kafelki, wygląd, PIN) trzymana wyłącznie w zamontowanym wolumenie, nigdy w repo ani w obrazie.
