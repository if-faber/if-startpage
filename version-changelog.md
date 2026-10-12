# Historia zmian — IF-startpage

Nowe wpisy dopisujemy na górze.

---

## v0.1.6.5 — 2026-10-12

Porządki w menu, progi alarmów, ikony Lucide i tryb dzień / noc w Zapleczu.

- **Menu ⚙️ na dole strony** zamiast trzech przycisków: „Dodaj link”, „Dodaj sekcję” (małe okno z jednym polem, sekcja trafia na bieżącą stronę) i „Zarządzanie” (Zaplecze → Strony i sekcje, od razu na bieżącej stronie).
- **Okno „Strony i sekcje” usunięte** ze Strony Głównej, razem z przyciskiem „Dodaj stronę” w menu i systemowym okienkiem `prompt()`. Strony dodaje się, przemianowuje, sortuje i usuwa w Zapleczu → **Strony i sekcje** (dawniej „Homepage”).
- **Pasek awarii** (`incident-bar`) — jedna klasa dla paska na Stronie Głównej, okienka dzwonka, widoku Alerty i Centrum alertów; pełne kolory niezależne od motywu (czerwony — alarm, pomarańczowy — uwaga). Przycisk ✕ chowa pasek; wraca przy nowym zgłoszeniu albo zmianie z „uwagi” na „alarm”. Alert zostaje w dzwonku.
- **Progi temperatur** (Zaplecze → Centrum alertów): tabela wszystkich czujników — obecna temperatura, próg „Uwaga” i „Alarm”, „Resetuj do domyślnych” i „Zapisz”. Domyślne progi wg rodzaju czujnika; własne w `config/temperature-thresholds.json`. Przekroczenie progu → alarm w pasku, dzwonku i Centrum alertów (wcześniej stały próg 78/85 °C tylko dla procesora).
- **Progi obciążenia CPU i RAM** (domyślnie 70/90 % i 80/95 %, `config/load-thresholds.json`) — kolor kafelków CPU i RAM w nagłówku, bez paska alarmowego. Kafelek TEMP usunięty z nagłówka.
- **Temperatury:** kafelek procesora bierze ogólną temperaturę CPU (Intel „Package id”, AMD „Tctl”), a nie pierwszy znaleziony czujnik (płyta główna). Statystyki Live mają kafelek „Temperatury” ze wszystkimi czujnikami; kolory wg progów.
- **Top:** naprawiona tabela procesów (kolumny czytane wg nagłówka `top`, nie wg stałych pozycji); kolory ramek zależne od motywu — czytelne w trybie dziennym.
- **Ikony Lucide zamiast emoji** w całym interfejsie Strony Głównej i Zaplecza (menu, tytuły, przyciski, nagłówki kafelków). Menu Zaplecza: „Helper & System” → **Ustawienia ogólne**, „Homepage” → **Strony i sekcje**; nazwy wyrównane do lewej.
- **Tryb dzień / noc w Zapleczu** — przełącznik w menu Zaplecza, tryb wspólny ze Stroną Główną. Wygląd Zaplecza zapisywany osobno dla dnia i nocy (`config/admin-theme.json`: `{ themes: { d, n } }`; dotychczasowy wygląd staje się nocnym). Jasny wygląd startowy dnia w `admin.css`; edytor „Wygląd Zaplecza” ma na górze wybór trybu.
- Naprawione: przyciski „Pauza” / „Odśwież” w panelu Top na Stronie Głównej nie miały stylu.

---

## v0.1.6.4 — 2026-10-12

- **Zaplecze → Homepage zna strony:** na górze wybór strony; sekcje i linki tej strony, ich kolejność strzałkami, dodawanie sekcji na wybraną stronę.
- **„Przenieś na stronę…”** przy każdej sekcji — przenosi sekcję z linkami na inną stronę (na jej koniec).

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
