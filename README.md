# IF-startpage

Samodzielna, lekka strona startowa domowego serwera: Strona Główna z kafelkami usług,
Zaplecze administracyjne chronione PIN-em i Kiosk statusu serwera.
Część pakietu **Idea Forge** dla serwerów domowych.

> **Nazwa:** projekt nazywa się `IF-startpage`, a nie „Homepage”, żeby nie mylić go
> z istniejącym projektem open source o tej nazwie (gethomepage.dev) ani z innymi
> dashboardami tej niszy (Homarr, Heimdall, Dashy, Homer, Flame).

## Relacja z IF-home-server

`IF-startpage` jest stroną startową serwera postawionego instalatorem
[`IF-home-server`](https://github.com/if-faber/IF-home-server) (Cockpit, Samba,
serwer druku, Dockge). Razem tworzą pakiet: strona startowa + panel administracyjny
+ udostępnianie plików + serwer druku.

Da się ją uruchomić samodzielnie na dowolnym Dockerze — w kodzie nie ma zależności
od `IF-home-server`. Wspólne są konwencje: katalogi `docker/app/` i `docker/app-data/`
oraz zarządzanie stosami przez Dockge.

## Funkcje

- **Strona Główna** — własne strony w menu (Home + dowolne, np. Ulubione), na nich sekcje
  z kafelkami linków, stałe strony serwera (Status, Top, Narzędzia, Alerty), tryb dzień/noc
  (przełącznik ☀️/🌙 na dole paska bocznego), menu ⚙️ w pasku bocznym (wygląd na żywo,
  Zaplecze, blokada edycji), menu ⚙️ na dole strony (dodaj link, dodaj sekcję, Zarządzanie),
  chowany pasek boczny, kafelki CPU i RAM w nagłówku (kolor wg progów) i dzwonek alertów.
  Awarie i przekroczone progi temperatur pokazuje pasek na górze strony (zamykany ✕).
- **Zaplecze** (chronione PIN-em, tryb dzień/noc wspólny ze Stroną Główną):
  - Ustawienia ogólne,
  - Centrum Alertów — stan kontenerów, temperatury, RAM, dyski, logi kontenerów;
    progi temperatur dla każdego czujnika i progi obciążenia CPU/RAM,
  - Statystyki Live — CPU, temperatury wszystkich czujników, RAM, dyski, sieć, procesy,
  - Strony i sekcje — dodawanie, nazwy, kolejność i usuwanie stron, sekcji i linków,
  - Aktualizacje kontenerów — porównuje wersje obrazów z najnowszymi stabilnymi
    i podmienia tag wersji w `compose.yaml` wybranego stosu,
  - Moduły (ustawienia Kiosku), edytor wyglądu Zaplecza (osobno dzień i noc).
- **Kiosk statusu serwera** — Status serwera, Live (zużycie per kontener),
  Status kontenerów, Radio.

## Wymagania

- Docker z Docker Compose (zalecane zarządzanie przez Dockge — zobacz `IF-home-server`).
- Zamontowany `/var/run/docker.sock` — dla Diagnostyki, Statusu kontenerów i Aktualizacji.
- Zamontowany katalog `docker/app/` hosta jako `/host-apps` — dla zakładki Aktualizacje
  (bez niego zmiana wersji kontenera zwróci błąd 404).

## Bezpieczeństwo — przeczytaj przed instalacją

Dostęp do `docker.sock` i zapis do `docker/app/` daje kontenerowi praktycznie pełną
kontrolę nad serwerem. Dlatego:

- **Zmień PIN** z domyślnego `1234` — w `compose.yaml` przed pierwszym uruchomieniem
  albo w Zapleczu (Ustawienia ogólne). PIN ustawiony w Zapleczu ma pierwszeństwo przed
  tym z `compose.yaml`.
- **Nie wystawiaj strony do internetu.** Jest przeznaczona do sieci domowej
  (z zewnątrz — przez VPN).
- Po 5 błędnych próbach PIN-u z jednego adresu Zaplecze blokuje się na minutę,
  przy kolejnych seriach coraz dłużej (maks. 15 minut).
- Bez PIN-u działa tylko podgląd: kafelki, statystyki serwera, lista kontenerów
  i alerty (tego potrzebuje Kiosk). Edycja kafelków, logi kontenerów, sprawdzanie
  portów i zmiany ustawień wymagają PIN-u.

## Obraz kontenera

Obraz jest budowany automatycznie przez GitHub Actions
(`.github/workflows/docker-publish.yml`) przy każdym tagu wersji `vX.Y.Z`
i publikowany w GitHub Container Registry:

```text
ghcr.io/if-faber/if-startpage:vX.Y.Z
ghcr.io/if-faber/if-startpage:latest
```

## Uruchomienie

Potrzebujesz terminala na serwerze (np. przez SSH albo Cockpit → Terminal) i Dockge
w przeglądarce. Polecenia kopiujesz i wklejasz **bez żadnych zmian**.

**Krok 1. Sprawdź swoją nazwę użytkownika.** W terminalu wpisz:

```bash
whoami
```

Pokaże się jedno słowo, np. `jan`. Zapisz je — przyda się w kroku 4.

**Krok 2. Utwórz foldery na dane.** Wklej w terminalu:

```bash
mkdir -p ~/docker/app-data/if-startpage/config ~/docker/app-data/if-startpage/backups ~/docker/app-data/if-startpage/icons
```

Sprawdź, czy są:

```bash
ls ~/docker/app-data/if-startpage/
```

Powinno się pokazać: `backups  config  icons`.

**Krok 3. Utwórz stos w Dockge.** W Dockge kliknij **+ Utwórz** (Compose), w polu nazwy
wpisz `if-startpage` i wklej całą zawartość pliku [`compose.yaml`](compose.yaml)
z tego repozytorium.

**Krok 4. Wpisz swoją nazwę użytkownika w compose.** W wklejonym tekście są miejsca
z napisem `{user}` (razem z nawiasami klamrowymi). Każde zamień na nazwę z kroku 1.

Przykład dla użytkownika `jan`:

```text
przed:  /home/{user}/docker/app-data/if-startpage/config:/app/config
po:     /home/jan/docker/app-data/if-startpage/config:/app/config
```

Dockge nie rozumie skrótu `~`, dlatego tutaj nazwę trzeba wpisać ręcznie.
W pliku nie może zostać żadne `{user}`.

**Krok 5. Ustaw własny PIN.** W linii `DASHBOARD_PIN=1234` zamień `1234` na swój PIN
(co najmniej 4 znaki). PIN chroni edycję strony i Zaplecze.

**Krok 6. Sprawdź port.** Strona działa na porcie 80 (linia `"80:3010"`). Jeśli na
serwerze port 80 jest już zajęty, zmień pierwszą liczbę, np. `"8090:3010"` — wtedy
adres strony będzie `http://adres-serwera:8090/`.

**Krok 7. Uruchom.** Kliknij **Wdróż** (Deploy). Strona będzie pod adresem
`http://adres-serwera/` (adres serwera to np. `192.168.1.10`).

### Ustawienia opcjonalne (zmienne środowiskowe)

- `DOCKER_VERSION_EXCLUDE` — kontenery pomijane przy sprawdzaniu wersji, po przecinku
  (np. obrazy budowane lokalnie).
- `DOCKER_STACK_OVERRIDES` — gdy nazwa kontenera różni się od nazwy stosu lub serwisu,
  w formacie JSON, np. `{"wordpress_db":{"stack":"wordpress","service":"db"}}`.

## Wygląd strony: dzień i noc

Wygląd startowy jest w `www/styles.css` (część obrazu, nie zmieniasz go).
Twoje zmiany z edytora wyglądu (menu ⚙️ w pasku bocznym → Wygląd) trafiają do dwóch zwykłych
plików CSS w katalogu konfiguracji:

- `config/custom-d.css` — tryb **dzień**,
- `config/custom-n.css` — tryb **noc**.

Na górze edytora wybierasz, który tryb edytujesz; „Zapisz wygląd” zapisuje plik tego trybu.
W plikach są tylko właściwości, dla których edytor ma kontrolki — każda z opisem po polsku.
Plik można poprawić ręcznie, ale kolejny zapis z edytora nadpisze go w całości.
„Wygląd startowy trybu” usuwa plik danego trybu. Tryb wybrany przełącznikiem zapamiętuje
przeglądarka.

Zaplecze używa tego samego trybu (przełącznik ☀️/🌙 jest też w menu Zaplecza). Jego wygląd
ustawia się w Zaplecze → Wygląd Zaplecza, osobno dla dnia i nocy — zapis trafia do
`config/admin-theme.json`. Kiosk ma jeden wygląd.

## Ikony aplikacji na kafelkach

W oknie linku w polu „Ikona” wpisujesz nazwę pliku, np. `immich.png`, `jellyfin.png`.

- Obraz zawiera ok. 160 popularnych ikon: aplikacje serwera domowego (Immich, Jellyfin,
  Home Assistant, Nextcloud, Dockge, Pi-hole, Vaultwarden i inne), popularne serwisy
  (Google, YouTube, Facebook, WhatsApp, Netflix, Amazon… — pełna lista w Zapleczu)
  i druk 3D (Printables, Prusa, OctoPrint, Klipper, Mainsail, Fluidd, OrcaSlicer, Cura, Spoolman).
  Logo Idea Forge: `idea-forge.webp`.
- Własne ikony przesyłasz w Zaplecze → Strony i sekcje → **Ikony aplikacji** („Prześlij ikony”)
  albo kopiujesz do folderu `~/docker/app-data/if-startpage/icons` na serwerze
  (PNG, SVG, WebP, JPG, ICO). Ikona o tej samej nazwie co wbudowana — zastępuje ją.
- **„Lista ikon”** w tym samym miejscu pokazuje wszystkie dostępne ikony z wyszukiwarką
  i przyciskiem „Kopiuj nazwę”.
- Wystarczy rozmiar ok. 128 × 128 px — na kafelku ikona ma 30 px.

W polu „Ikona” można też wpisać jedną literę albo adres obrazka z internetu.

### Gdzie szukać ikon

| Źródło | Licencja | Co zawiera |
|---|---|---|
| [dashboard-icons](https://github.com/homarr-labs/dashboard-icons) (Homarr) | Apache 2.0 | ponad 2500 kolorowych ikon aplikacji i serwisów — z tego pakietu pochodzą ikony wbudowane |
| [selfh.st/icons](https://selfh.st/icons-about/) | CC BY 4.0 (podaj autora) | ponad 7000 kolorowych ikon, PNG / WebP / SVG, wersje jasne i ciemne |
| [Simple Icons](https://simpleicons.org) | CC0 | ponad 3300 marek, jednokolorowe SVG (m.in. Allegro, Creality) |
| [SVG Logos](https://github.com/gilbarbara/logos) (Gil Barbara) | CC0 | ok. 1900 kolorowych logo, głównie technologie |
| Ikona samej strony | — | strony udostępniają ikonę dla telefonów (`apple-touch-icon`, zwykle 180 × 180 px) — dobre źródło dla polskich serwisów, których nie ma w pakietach |

Logotypy są znakami towarowymi swoich właścicieli — używaj ich tylko do oznaczenia kafelków
prowadzących do tych usług.

## Progi alarmów

W Zaplecze → Centrum alertów:

- **Progi temperatur** — dla każdego czujnika próg „Uwaga” (pomarańczowy) i „Alarm”
  (czerwony). Każdy komputer ma inne normy, więc progi ustawia się dla swojego serwera
  (`config/temperature-thresholds.json`). Przekroczenie progu pokazuje pasek na Stronie Głównej,
  dzwonek i Centrum alertów.
- **Progi obciążenia** — CPU i RAM zmieniają tylko kolor kafelków w nagłówku. Wysokie
  obciążenie to zwykle praca (np. OCR, galeria zdjęć), a nie awaria, więc bez paska.
  RAM powyżej 95 % trafia dodatkowo do dzwonka (`config/load-thresholds.json`).

## Znane ograniczenia

- **Aktualizacje pakietów systemowych (apt)** nie są sprawdzane z wnętrza kontenera —
  kontener nie widzi pakietów serwera. Zakładka odsyła do Cockpit (port 9090).
- **Dyski** w Diagnostyce pokazują to, co widzi kontener — zwykle dysk systemowy.
- **Procesy** w Top / Statystykach Live to procesy kontenera, nie całego serwera
  (kontener ma własną przestrzeń procesów).

## Dokumentacja i pytania

Pełna dokumentacja krok po kroku, ze zdjęciami, jest w przygotowaniu.
Coś jest niejasne albo nie działa? Zgłoś to w [Issues](../../issues).
Odpowiadam zwykle raz w tygodniu.

## Podziękowania

Ikony usług pochodzą z projektu
[dashboard-icons](https://github.com/homarr-labs/dashboard-icons).
Szczegóły i licencje materiałów osób trzecich: [THIRD-PARTY.md](THIRD-PARTY.md).

## Licencja

MIT — zobacz [LICENSE](LICENSE).

## Historia zmian

[version-changelog.md](version-changelog.md)
