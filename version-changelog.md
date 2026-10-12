# Historia zmian — IF-startpage

Nowe wpisy dopisujemy na górze.

---

## v0.1.4 — 2026-09-18

Pierwsza publiczna wersja IF-startpage: samodzielna strona startowa serwera domowego, działająca w kontenerze Docker.

- **Strona Główna:** kafelki linków w sekcjach, motyw dzień/noc, chowany pasek boczny, widżet CPU/RAM/temperatury w nagłówku.
- **Zaplecze (chronione PIN-em):** Centrum Alertów, Diagnostyka Serwera (Top), Statystyki Live, Aktualizacje kontenerów, Helper & System, edytor wyglądu.
- **Kiosk statusu serwera:** Status serwera, Live (zużycie per kontener), Status kontenerów, Radio.
- **Obraz kontenera** budowany automatycznie przez GitHub Actions i publikowany w `ghcr.io/if-faber/if-startpage`.
- Konfiguracja użytkownika (kafelki, wygląd, PIN) trzymana wyłącznie w zamontowanym wolumenie, nigdy w repo ani w obrazie.
