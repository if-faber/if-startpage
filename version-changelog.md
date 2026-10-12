# Historia zmian — IF-startpage

Nowe wpisy dopisujemy na górze.

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
