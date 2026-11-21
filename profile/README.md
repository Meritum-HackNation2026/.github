<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="meritum-mark-on-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="meritum-mark.svg">
    <img src="meritum-mark.svg" alt="Znak Meritum" width="72" height="72">
  </picture>
</p>

<h1 align="center">Meritum</h1>

<p align="center">Meritum — obywatelski radar dezinformacji</p>

Meritum to prototyp przygotowany na hackathon HackNation 2026 w konkursie MSZ „Prawdowid”. Meritum pozwala sprawdzić pogłoskę albo informację tak łatwo, jak wysłać wiadomość: wklejasz tekst lub link i dostajesz ocenę każdego twierdzenia razem ze źródłami, na których się opiera.

## Problem

Obywatel trafia na pogłoskę w feedzie albo w wiadomości od znajomych. Sprostowanie często już istnieje, ale w innym miejscu: w serwisie fact-checkingowym, w komunikacie urzędu, w akcie prawnym. Generatywna AI odpowiada płynnie i pewnie, ale płynność nie jest dowodem, a odpowiedź bez źródła trudno sprawdzić. Część treści jest prawdziwa, ale nieaktualna, część łączy poprawne dane z błędnym wnioskiem, a o części nikt jeszcze rzetelnie nie napisał.

## Jak to działa

1. **Kilka kanałów, jedna logika.** Podstawowym kanałem jest formularz WWW: wklejasz tekst albo link. Konto nie jest potrzebne; logowanie kontem Google albo Microsoft jest opcjonalne i pozwala zachować historię. Drugim kanałem jest przycisk „Sprawdź” we wtyczce Chrome pod postem na platformie społecznościowej. Ten sam rdzeń obsługuje bota na Discordzie, a jeśli starczy czasu, także SMS. Wszystkie kanały wysyłają zapytanie do tego samego rdzenia weryfikacji, więc to samo pytanie daje ten sam wynik.
2. **Twierdzenia.** System wyodrębnia z treści twierdzenia, które da się sprawdzić, i oddziela je od opinii.
3. **Źródła.** Szuka w opisanym i audytowalnym korpusie: serwisy urzędowe z listy organizatora oraz ustalenia fact-checkerów z Google Fact Check Tools API. Świeże publikacje dociąga wyszukiwaniem w sieci, ale tylko w domenach z katalogu źródeł. Każdy dokument ma datę publikacji i datę pobrania.
4. **Werdykt.** Dla każdego twierdzenia jeden z pięciu wyników: potwierdzone, fałszywe, wprowadzające w błąd, brak wystarczających dowodów albo nie podlega weryfikacji. Awaria techniczna, prośba o doprecyzowanie i pytanie poza zakresem nie są werdyktami i są komunikowane osobno.
5. **Cytaty.** Każde ustalenie prowadzi do źródła: tytuł, wydawca, data, link i dosłowny fragment. Cytatu, którego nie ma w tekście źródła, system odrzuca. Wyjaśnienie modelu jest oddzielone od materiału źródłowego.
6. **Siła podstawy.** Wynika z jawnych przesłanek: liczby niezależnych źródeł, udziału źródeł urzędowych, ich aktualności i zgodności. Sprzeczne źródła są pokazywane jako rozbieżność, bez arbitralnej pewności.
7. **Wstrzymanie się.** Gdy dowodów brakuje, system nie rozstrzyga i mówi, czego nie udało się ustalić.
8. **Moderatorzy.** Sprawy nierozstrzygnięte i powtarzające się pytania bez odpowiedzi trafiają do kolejki moderatorów jako jedna sprawa na grupę podobnych zgłoszeń. System odróżnia zalecenie przekazania od faktycznego przekazania.
9. **Bezpieczeństwo i ślad.** Instrukcje znalezione w sprawdzanych treściach nie zmieniają zasad działania: są ignorowane i zapisywane jako zdarzenie. Każde sprawdzenie ma zapis przebiegu (pytanie z zamaskowanymi danymi osobowymi, czas, źródła, wynik, użyty model, koszt) z określonym terminem usunięcia.

AI pomaga zrozumieć pytanie, przeszukać i porównać materiały oraz napisać odpowiedź prostym językiem. Nie rozstrzyga sama: werdykt wynika z cytatów i przesłanek, które użytkownik może sprawdzić.

## Wyzwanie HackNation 2026

MSZ „Prawdowid”: szybka i łatwa weryfikacja informacji oparta na wiarygodnych źródłach. Narzędzie dla obywatela, także o niskich kompetencjach cyfrowych, które pokazuje podstawy odpowiedzi i uczciwie wstrzymuje się od rozstrzygnięcia, gdy materiały są niewystarczające.

Projekt nie ma partnerstw z instytucjami organizującymi wyzwanie. Dane w makietach są fikcyjne. Wyniki testów opublikujemy razem z danymi, które pozwalają je odtworzyć.

## Technologie

- Backend: Python, FastAPI, FastMCP, Pydantic, Pydantic AI, DBOS (trwałe workflowy)
- Baza danych: PostgreSQL z pgvector
- Wyszukiwanie w sieci: wymienny adapter ograniczony do domen z katalogu źródeł (planowany Firecrawl)
- Frontend: React, TanStack Router, TanStack Query, shadcn/ui, Tailwind CSS v4
- Wtyczka do przeglądarki Chrome
- Hosting: na czas prac serwer zespołu za Cloudflare; docelowo GCP w UE (aplikacja w europe-central2, modele z Vertex AI)

Model językowy i dostawcę wyszukiwania można wymienić bez przebudowy systemu. Ten sam rdzeń jest dostępny przez API oraz jako serwer MCP.

## Działające środowiska

`prd` to wersja z gałęzi `main`, `dev` z gałęzi `develop`. Na razie to szkielety aplikacji: działa wdrożenie, baza danych i kolejka zadań, a funkcje sprawdzania dopiero powstają.

### Aplikacja webowa i API

| Nazwa | Wersja | Przeznaczenie | Link |
|---|---|---|---|
| Aplikacja webowa | prd | aplikacja dla obywateli (bootstrap: stan API; docelowo formularz sprawdzania twierdzeń) | [meritum.gburek.app](https://meritum.gburek.app) |
| Aplikacja webowa | dev | aplikacja dla obywateli (bootstrap: stan API; docelowo formularz sprawdzania twierdzeń) | [meritum-dev.gburek.app](https://meritum-dev.gburek.app) |
| API | prd | dokumentacja OpenAPI (Swagger UI) | [meritum.gburek.app/api/docs](https://meritum.gburek.app/api/docs) |
| API | dev | dokumentacja OpenAPI (Swagger UI) | [meritum-dev.gburek.app/api/docs](https://meritum-dev.gburek.app/api/docs) |
| API: stan | prd | health: API, baza danych i środowisko | [meritum.gburek.app/api/v1/health](https://meritum.gburek.app/api/v1/health) |
| API: stan | dev | health: API, baza danych i środowisko | [meritum-dev.gburek.app/api/v1/health](https://meritum-dev.gburek.app/api/v1/health) |

### Makiety platform

| Nazwa | Wersja | Przeznaczenie | Link |
|---|---|---|---|
| Makieta X | prd | makieta platformy do demo wtyczki | [meritum-x.gburek.app](https://meritum-x.gburek.app) |
| Makieta X | dev | makieta platformy do demo wtyczki | [meritum-x-dev.gburek.app](https://meritum-x-dev.gburek.app) |
| Makieta Facebook | prd | makieta platformy do demo wtyczki | [meritum-facebook.gburek.app](https://meritum-facebook.gburek.app) |
| Makieta Facebook | dev | makieta platformy do demo wtyczki | [meritum-facebook-dev.gburek.app](https://meritum-facebook-dev.gburek.app) |
| Makieta Instagram | prd | makieta platformy do demo wtyczki | [meritum-instagram.gburek.app](https://meritum-instagram.gburek.app) |
| Makieta Instagram | dev | makieta platformy do demo wtyczki | [meritum-instagram-dev.gburek.app](https://meritum-instagram-dev.gburek.app) |
| Makieta Reddit | prd | makieta platformy do demo wtyczki | [meritum-reddit.gburek.app](https://meritum-reddit.gburek.app) |
| Makieta Reddit | dev | makieta platformy do demo wtyczki | [meritum-reddit-dev.gburek.app](https://meritum-reddit-dev.gburek.app) |
| Makieta TikTok | prd | makieta platformy do demo wtyczki | [meritum-tiktok.gburek.app](https://meritum-tiktok.gburek.app) |
| Makieta TikTok | dev | makieta platformy do demo wtyczki | [meritum-tiktok-dev.gburek.app](https://meritum-tiktok-dev.gburek.app) |

### Wtyczka Chrome

| Nazwa | Wersja | Przeznaczenie | Link |
|---|---|---|---|
| Wtyczka Chrome | prd | zip do załadowania w chrome://extensions (artefakt `meritum-extension-prd` w przebiegu CI) | [przebiegi CI na `main`](https://github.com/Meritum-HackNation2026/meritum-extension/actions/workflows/ci.yml?query=branch%3Amain+is%3Asuccess) |
| Wtyczka Chrome | dev | zip do załadowania w chrome://extensions (artefakt `meritum-extension-dev` w przebiegu CI) | [przebiegi CI na `develop`](https://github.com/Meritum-HackNation2026/meritum-extension/actions/workflows/ci.yml?query=branch%3Adevelop+is%3Asuccess) |

### Discord

| Nazwa | Wersja | Przeznaczenie | Link |
|---|---|---|---|
| Discord: Meritum | prd | dodanie aplikacji do konta albo serwera; komendy /sprawdz i „Sprawdź w Meritum” przy wiadomości | [zainstaluj „Meritum”](https://discord.com/oauth2/authorize?client_id=1573692006398693396) |
| Discord: Meritum dev | dev | dodanie aplikacji do konta albo serwera; komendy /sprawdz i „Sprawdź w Meritum” przy wiadomości | [zainstaluj „Meritum dev”](https://discord.com/oauth2/authorize?client_id=1573690220782559332) |

## Repozytoria

Repozytoria są na razie prywatne, więc linki działają dla członków organizacji. Na koniec prac cały kod będzie publiczny, z instrukcją uruchomienia przez `docker compose up`.

| Repozytorium | Zawartość |
|---|---|
| [meritum-backend](https://github.com/Meritum-HackNation2026/meritum-backend) | API w FastAPI; zleca długie operacje workerom przez DBOS |
| [meritum-worker](https://github.com/Meritum-HackNation2026/meritum-worker) | workery DBOS do długich operacji w tle, na PostgreSQL |
| [meritum-core](https://github.com/Meritum-HackNation2026/meritum-core) | kod wspólny backendu i workerów: kontrakty (jedyne źródło prawdy dla kontraktów danych), porty, adaptery |
| [meritum-frontend](https://github.com/Meritum-HackNation2026/meritum-frontend) | aplikacja webowa (React, pnpm), serwowana przez nginx |
| [meritum-extension](https://github.com/Meritum-HackNation2026/meritum-extension) | wtyczka Chrome MV3 (WXT, React) |
| [meritum-mocks](https://github.com/Meritum-HackNation2026/meritum-mocks) | makiety X, Facebooka, Instagrama, Reddita i TikToka do demo wtyczki |
| [meritum-marketing](https://github.com/Meritum-HackNation2026/meritum-marketing) | deck, film i grafiki na konkurs MSZ |
| [meritum-design-system](https://github.com/Meritum-HackNation2026/meritum-design-system) | dokument produktowy, system projektowy, teksty, źródłowe makiety |
| [meritum-infra](https://github.com/Meritum-HackNation2026/meritum-infra) | runnery GitHub Actions i wspólna platforma na serwerze zespołu |
| [meritum-monorepo](https://github.com/Meritum-HackNation2026/meritum-monorepo) | repozytorium zbiorcze, łączy pozostałe jako submoduły git |

## English summary

Meritum is a civic disinformation radar, built as a prototype for HackNation 2026 in the MSZ (Polish Ministry of Foreign Affairs) "Prawdowid" challenge. A citizen pastes text or a link into a web form, or clicks "Sprawdź" (Check) under a post in the Chrome extension; a Discord bot, and SMS if time allows, use the same verification core. The system extracts checkable claims, searches an audited corpus of official sites from the organiser's list and fact-checks from the Google Fact Check Tools API, plus live web search limited to catalogue domains, and returns one of five verdicts per claim: confirmed, false, misleading, insufficient evidence, or not verifiable. Every finding cites a verbatim fragment with title, publisher, date and link, and the strength of the verdict follows from explicit premises. When evidence is missing, the system abstains and says what it could not establish; unresolved and repeated questions go to a moderator queue. Instructions found in checked content are ignored and logged, and every check leaves an audit record. Code repositories are private for now; all repositories and the system go public at the end of development, with run instructions using one `docker compose up`. Working dev and prd links are listed above.
