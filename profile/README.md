<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="meritum-mark-on-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="meritum-mark.svg">
    <img src="meritum-mark.svg" alt="Znak Meritum" width="72" height="72">
  </picture>
</p>

<h1 align="center">Meritum</h1>

<p align="center">Meritum — obywatelski radar dezinformacji oraz scamu</p>

Meritum to prototyp przygotowany na hackathon HackNation 2026. Działa jako wtyczka do przeglądarki na platformach społecznościowych (X, Facebook, Instagram, Reddit, TikTok) i dodaje pod postami oraz reklamami kontekst, który powstaje niezależnie od samych platform.

## Problem

Manipulacyjny post i fałszywa reklama z wizerunkiem znanej osoby mają podobną budowę. Pojedyncza treść jest zwykle fragmentem większej operacji: wiele kopii, krótki czas życia, skoordynowane konta i domeny, które szybko się zmieniają. Sprostowanie, jeśli w ogóle powstaje, trafia gdzie indziej niż czytelnik. Zgłoszona reklama znika, a następnego dnia wraca pod nowym adresem i razem z nią przepadają ślady.

## Jak to działa

1. Użytkownik zgłasza w kilku kliknięciach manipulację albo podejrzaną reklamę.
2. System porównuje zgłoszenie z wcześniejszymi (podobieństwo obrazu i tekstu, szablon strony docelowej, domeny i przekierowania) i dołącza je do kampanii, do której należy.
3. AI przygotowuje szkic notatki kontekstowej z uwag użytkowników, analizy treści i źródeł z różnych stron sporu. Linki do źródeł są obowiązkowe, a notatka bez źródła nie może zostać opublikowana.
4. Notatka ukazuje się dopiero wtedy, gdy za pomocną uznają ją osoby z grup, które zwykle oceniają treści inaczej. Sama większość głosów nie wystarcza, więc farma botów nie może jej przepchnąć ani zakopać.
5. Na fałszywej reklamie wtyczka pokazuje ostrzeżenie o oszustwie i kampanię, z której pochodzi. W chwili zobaczenia reklamy zapisuje dowód: zrzut ekranu, adres, łańcuch przekierowań i czas. Dowód zostaje także wtedy, gdy reklama zniknie.

AI pisze szkic, a o publikacji decydują ludzie. Notatki wyjaśniają i cytują źródła, nie oceniają ludzi.

## Wyzwania HackNation 2026

Ten sam produkt odpowiada na dwa wyzwania:

- MSZ „Prawdowid”: narzędzie AI, które pomaga odróżnić fakt od manipulacji, dotrzeć do wiarygodnych źródeł i świadomie oceniać napotykane treści. Meritum odpowiada notatką kontekstową z obowiązkowymi źródłami, pokazaniem, z której strony sporu pochodzą media, i nazwaniem techniki manipulacji.
- NASK „Kolekcjoner fejków”: fałszywe reklamy z wizerunkiem znanych osób szybko znikają, zmieniają formę i wracają pod nowymi adresami, a pojedynczy materiał bywa fragmentem dużo większej kampanii. Meritum łączy zgłoszenia w kampanie, pokazuje ich warianty i domeny na jednej osi czasu i zachowuje dowody, zanim reklama zniknie.

Projekt nie ma partnerstw z instytucjami organizującymi wyzwania. Wszystkie liczby pokazywane w demo są symulowane.

## Technologie

- Backend: Python, FastAPI, FastMCP, Pydantic, Pydantic AI, DBOS (trwałe workflowy)
- Baza danych: PostgreSQL z pgvector
- Frontend: React, TanStack Router, TanStack Query, shadcn/ui, Tailwind CSS v4
- Wtyczka do przeglądarki Chrome
- Chmura: GCP i Cloudflare

Ten sam silnik weryfikacji ma być dostępny przez API oraz jako serwer MCP, żeby redakcje i analitycy mogli z niego korzystać bez instalowania wtyczki.

## Repozytoria

Repozytoria z kodem są na razie prywatne.

- `meritum-design-system`: system projektowy, znak, tokeny, komponenty, teksty interfejsu i makiety platform do demo.
- `meritum-infra`: infrastruktura i wdrożenie.
- `meritum-monorepo`: repozytorium zbiorcze, które łączy pozostałe jako submoduły git.

## English summary

Meritum is a civic radar for disinformation and scams, built as a prototype for HackNation 2026. A browser extension lets people report manipulative posts and fake ads on social platforms. Each report is linked to the wider campaign it belongs to. An AI drafts a context note that must cite sources, and the note is published only when people who usually disagree rate it helpful. Fake ads get a scam warning, and the extension preserves evidence (screenshot, URL, redirect chain, time) before the ad disappears. The project targets two challenges: MSZ "Prawdowid" (fact versus manipulation, credible sources) and NASK "Kolekcjoner fejków" (fake celebrity ads that vanish and return as part of larger campaigns). Code repositories are private for now.
