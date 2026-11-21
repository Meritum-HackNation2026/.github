<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="meritum-mark-on-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="meritum-mark.svg">
    <img src="meritum-mark.svg" alt="Znak Meritum" width="72" height="72">
  </picture>
</p>

<h1 align="center">Meritum</h1>

<p align="center">Meritum — obywatelski radar dezinformacji</p>

Meritum to prototyp przygotowany na hackathon HackNation 2026 w konkursie MSZ „Prawdowid”. Pozwala sprawdzić pogłoskę albo doniesienie tak łatwo, jak wysłać wiadomość: wklejasz tekst lub link i dostajesz ocenę każdego twierdzenia razem ze źródłami, na których się opiera.

## Problem

Obywatel trafia na pogłoskę w feedzie albo w wiadomości od znajomych. Sprostowanie często już istnieje, ale w innym miejscu: w serwisie fact-checkingowym, w komunikacie urzędu, w akcie prawnym. Generatywna AI odpowiada płynnie i pewnie, ale płynność nie jest dowodem, a odpowiedź bez źródła trudno sprawdzić. Część treści jest prawdziwa, ale nieaktualna, część łączy poprawne dane z błędnym wnioskiem, a o części nikt jeszcze rzetelnie nie napisał.

## Jak to działa

1. **Dwa kanały, jedna logika.** Podstawowym kanałem jest formularz WWW: wklejasz tekst albo link. Drugim jest przycisk „Sprawdź” we wtyczce Chrome pod postem na platformie społecznościowej. Oba kanały wysyłają zapytanie do tego samego rdzenia weryfikacji, więc to samo pytanie daje ten sam wynik.
2. **Twierdzenia.** System wyodrębnia z treści twierdzenia, które da się sprawdzić, i oddziela je od opinii.
3. **Źródła.** Szuka w opisanym i audytowalnym korpusie: dokumenty urzędowe, dane publiczne i publikacje organizacji fact-checkingowych. Świeże publikacje dociąga wyszukiwaniem w sieci, ale tylko w domenach z katalogu źródeł. Każdy dokument ma datę publikacji i datę pobrania.
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
- Chmura: GCP (region europe-central2) i Cloudflare

Model językowy i dostawcę wyszukiwania można wymienić bez przebudowy systemu. Ten sam rdzeń jest dostępny przez API oraz jako serwer MCP.

## Repozytoria

Repozytoria są na razie prywatne. Repozytoria z kodem otworzymy dla jury.

- `meritum-design-system`: system projektowy, znak, tokeny, komponenty, kontrakty danych, teksty interfejsu i makiety platform do demo.
- `meritum-core`: wspólne kontrakty zadań, porty i adaptery backendu i workerów.
- `meritum-infra`: infrastruktura i wdrożenie.
- `meritum-monorepo`: repozytorium zbiorcze, które łączy pozostałe jako submoduły git.
- `meritum-backend`, `meritum-worker`, `meritum-extension`: utworzone, jeszcze puste.

## English summary

Meritum is a civic disinformation radar, built as a prototype for HackNation 2026 in the MSZ (Polish Ministry of Foreign Affairs) "Prawdowid" challenge. A citizen pastes text or a link into a web form, or clicks "Sprawdź" (Check) under a post in the Chrome extension; both channels use the same verification core. The system extracts checkable claims, searches an audited corpus of official documents, public data and fact-checks, plus live web search limited to catalogue domains, and returns one of five verdicts per claim: confirmed, false, misleading, insufficient evidence, or not verifiable. Every finding cites a verbatim fragment with title, publisher, date and link, and the strength of the verdict follows from explicit premises. When evidence is missing, the system abstains and says what it could not establish; unresolved and repeated questions go to a moderator queue. Instructions found in checked content are ignored and logged, and every check leaves an audit record. Code repositories are private for now and will be opened for the jury.
