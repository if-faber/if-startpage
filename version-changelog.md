# Historia zmian — IF-startpage

Nowe wpisy dopisujemy na górze.

---

## v0.1.6.3 — 2026-10-12

Strony w menu i ikony zamiast emoji.

- **Strony:** w menu na górze Home i dodane strony. „+ Dodaj stronę” pyta tylko o nazwę, strona od razu pojawia się w menu. Sekcje są nagłówkami grup kafelków na stronie.
- **Home** można przemianować; nie można go usunąć. Usunięcie innej strony nie kasuje kafelków — jej sekcje przechodzą na Home. Obecne sekcje trafiają automatycznie na Home.
- **Stałe strony serwera** pod kreską: Status serwera, Top (procesy), Narzędzia (test portów), Alerty — wcześniej wszystko było na jednym widoku.
- **Przyciski menu w stylu „lizak”** (jak w Kiosku): okrągła główka z ikoną i patyczek z nazwą.
- **Ikony Lucide** (licencja ISC) zamiast emoji w menu, przy dzwonku i ⚙️.
- Okno „Strona i sekcje” (przycisk na dole i w menu ⚙️): nazwa strony, kolejność stron, usuwanie, sekcje tej strony.
- Edycja i usuwanie kafelka bez odblokowania najpierw pyta o PIN.
- Sekcje dodane w Zapleczu (zakładka Homepage) trafiają na Home.

---

## v0.1.6.2 — 2026-10-12

Nowa ikona i uporządkowane menu Strony Głównej.

- **Ikona IF-startpage** (dom z kafelkami w kolorach Idea Forge): logo w nagłówku, favicon, ikona na telefonie. Źródło: `www/images/if-startpage.svg`.
- **Menu na dole paska bocznego:** zamiast pięciu ikon bez podpisów — przełącznik ☀️/🌙 i jeden przycisk ⚙️ z rozwijanym menu z podpisami: Wygląd (na żywo), Sekcje i kafelki, Zaplecze, Zablokuj/Odblokuj edycję.
- **Dzwonek alertów** przeniesiony do nagłówka, obok wyszukiwarki.
- **Stopka** „created by Idea Forge” pod menu.
- **Przyciski „+ Dodaj sekcję” i „+ Dodaj link”** zawsze na samym dole widoku (przyklejone do dołu przy przewijaniu). Bez odblokowanej edycji najpierw pytają o PIN.
- Podpowiedź na pustej stronie wskazuje przycisk „+ Dodaj sekcję”.
- Naprawione: manifest aplikacji odsyłał do nieistniejącego pliku `logo-512.png`.

---

## v0.1.6.1 — 2026-10-12

Tryb dzień / noc na Stronie Głównej.

- **Przełącznik ☀️/🌙** na dole paska bocznego (ikona z projektu theme-toggles, MIT). Wybór zapamiętuje przeglądarka; domyślnie noc.
- **Wygląd w plikach CSS:** zmiany z edytora 🎨 zapisują się do `config/custom-d.css` (dzień) albo `config/custom-n.css` (noc). Wygląd startowy zostaje w `styles.css`.
- **Wybór trybu na górze edytora** — edytor wie, który plik zmienia; „Zapisz wygląd” zapisuje plik wybranego trybu, „Wygląd startowy trybu” go usuwa.
- **Wygląd startowy dnia i nocy** w `custom-defaults/custom-d.css` i `custom-n.css` — dopracowane kolory, niższe kafelki, spokojniejsze ramki.
- Nowe kontrolki: kolor tekstu, kolor tekstu pomocniczego, tekst na przyciskach w kolorze akcentu.
- **Naprawione:** „Anuluj” w edytorze strony zostawiał niezapisane zmiany na ekranie do przeładowania.
- Zgodność: dotychczasowy zapis wyglądu (`theme.json`) działa w trybie noc do pierwszego zapisu nocy, potem zostaje jako `theme.json.old`.

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
