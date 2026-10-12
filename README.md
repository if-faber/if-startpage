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

- **Strona Główna** — kafelki linków w sekcjach, tryb dzień/noc (przełącznik ☀️/🌙 na dole
  paska bocznego), chowany pasek boczny, widżet CPU/RAM/temperatury w nagłówku.
- **Zaplecze** (chronione PIN-em):
  - Centrum Alertów — stan kontenerów, temperatura, RAM, dyski, logi kontenerów,
  - Diagnostyka Serwera (Top) i Statystyki Live — CPU, RAM, dyski, sieć, procesy,
  - Aktualizacje kontenerów — porównuje wersje obrazów z najnowszymi stabilnymi
    i podmienia tag wersji w `compose.yaml` wybranego stosu,
  - Helper & System, edytor wyglądu, ustawienia Kiosku.
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
  albo w Zapleczu (Helper & System). PIN ustawiony w Zapleczu ma pierwszeństwo przed
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

W komendach i w `compose.yaml` poniżej:

`{user}` — twoja nazwa użytkownika (sprawdzisz ją komendą `whoami`)

1. Utwórz foldery na dane trwałe:

   ```bash
   mkdir -p /home/{user}/docker/app-data/if-startpage/config
   mkdir -p /home/{user}/docker/app-data/if-startpage/backups
   ```

2. W Dockge utwórz nowy stos `if-startpage` i wklej zawartość pliku
   [`compose.yaml`](compose.yaml) z tego repo.
3. Zamień w nim każde `{user}` na swoją nazwę użytkownika i ustaw własny `DASHBOARD_PIN`.
4. Wdróż stos. Strona będzie pod adresem `http://adres-serwera/`.

Pełne ścieżki (`/home/{user}/...`) zamiast względnych (`../../...`) zawsze wskazują
właściwe miejsce, niezależnie od tego, jak Dockge widzi swój katalog stosów.

### Ustawienia opcjonalne (zmienne środowiskowe)

- `DOCKER_VERSION_EXCLUDE` — kontenery pomijane przy sprawdzaniu wersji, po przecinku
  (np. obrazy budowane lokalnie).
- `DOCKER_STACK_OVERRIDES` — gdy nazwa kontenera różni się od nazwy stosu lub serwisu,
  w formacie JSON, np. `{"wordpress_db":{"stack":"wordpress","service":"db"}}`.

## Wygląd strony: dzień i noc

Wygląd startowy jest w `www/styles.css` (część obrazu, nie zmieniasz go).
Twoje zmiany z edytora wyglądu (🎨 na dole paska bocznego) trafiają do dwóch zwykłych
plików CSS w katalogu konfiguracji:

- `config/custom-d.css` — tryb **dzień**,
- `config/custom-n.css` — tryb **noc**.

Na górze edytora wybierasz, który tryb edytujesz; „Zapisz wygląd” zapisuje plik tego trybu.
W plikach są tylko właściwości, dla których edytor ma kontrolki — każda z opisem po polsku.
Plik można poprawić ręcznie, ale kolejny zapis z edytora nadpisze go w całości.
„Wygląd startowy trybu” usuwa plik danego trybu. Tryb wybrany przełącznikiem zapamiętuje
przeglądarka. Na razie dotyczy to Strony Głównej; Zaplecze i Kiosk mają jeden wygląd.

## Znane ograniczenia

- **Aktualizacje pakietów systemowych (apt)** nie są sprawdzane z wnętrza kontenera —
  kontener nie widzi pakietów serwera. Zakładka odsyła do Cockpit (port 9090).
- **Dyski** w Diagnostyce pokazują to, co widzi kontener — zwykle dysk systemowy.

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
