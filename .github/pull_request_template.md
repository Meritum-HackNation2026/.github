Closes #

## Co i dlaczego

<!-- Jedno lub dwa zdania. Kontekst biznesowy jest w zgłoszeniu; nie powtarzaj go. Do tego najmniejszy widok, który pokazuje zmianę: drzewo wywołań, drzewo plików albo szkic diff (skill `mattpocock-skills:pr`). -->

## Dowód

<!-- Przed i po: test, który bez zmiany nie przechodzi, a z nią przechodzi; wynik polecenia; zrzuty ekranu jasny/ciemny dla UI. -->

## Ryzyko scalenia

Drzwi: <!-- dwukierunkowe (łatwo wycofać) albo jednokierunkowe (migracja, usuwanie danych, zmiana kontraktu) -->
Zasięg: <!-- jedno słowo, np. lokalny, kontrakt, kanały; plus kogo dotknie -->

## Jak sprawdzone

- [ ] Python: `ruff format --check`, `ruff check`, `ty check`, `pytest`, `behave`, `vulture`, `scripts/check_conventions.py`
- [ ] Python: `bandit` (repozytoria Python uruchamiają go obok reguł ruff `S`)
- [ ] TypeScript: `biome ci`, `tsc --noEmit`, `pnpm test` (`node --test`, w tym `tests/architecture/`), `knip`, dependency-cruiser
- [ ] Ręcznie: <!-- co kliknięto / które wywołanie API, albo "nie dotyczy" -->

## Lista kontrolna

- [ ] Zgłoszenie jest podlinkowane (`Closes #n` albo `Closes Meritum-HackNation2026/<repo>#n`) i każdy punkt z „Criteria of success” jest spełniony albo opisany niżej
- [ ] Nowe zachowanie ma testy (scenariusz behave z Gherkina zgłoszenia, pytest albo `node --test`), pisane jako czerwone przed kodem
- [ ] Publiczne klasy i funkcje mają docstringi/JSDoc z parametrami, wynikiem, błędami i sekcją `Features:` / `@feature` z ID historii i funkcji
- [ ] Reguły architektury (skill `meritum-architecture`): pionowe wycinki, prywatne nazwy z `_`, porty i adaptery z fabryką, pliki do 250 linii, stałe w `constants`, pełne typy; każdy wyjątek od lintera ma powód w komentarzu
- [ ] Zmiany w UI (skill `meritum-ui`): tokeny, kroje, komponenty i teksty z design systemu, werdykty nie tylko kolorem, WCAG 2.2 AA w trybie jasnym i ciemnym; zrzuty ekranu jasny/ciemny w sekcji „Dowód”
- [ ] Samoprzegląd skillem `mattpocock-skills:code-review` (osie Standards i Spec) zrobiony przed otwarciem PR
- [ ] Dokumentacja i README zaktualizowane, jeśli zmieniło się zachowanie albo konfiguracja
- [ ] CI nie dostało testów e2e ani testów wymagających kontaktu komponentów (backend ↔ worker, frontend ↔ backend, wspólna baza, przeglądarka); takie testy uruchamia się ręcznie albo lokalnie skryptem
- [ ] Bez podpisów AI w commitach i opisie PR

## Odstępstwa od design systemu

<!-- Tylko skórki platform (widżet, makiety) są dozwolone bez uzasadnienia. Każde inne odstępstwo: co i dlaczego. Albo "brak". -->

## Przegląd

Runda: <!-- review: round 1 | 2 | 3 (etykieta na PR) -->
