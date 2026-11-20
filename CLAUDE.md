# ai1st.cz – kontext projektu

Web AI automatizační agentury **ai1st.cz** (Filip Šerý a Vojta Pokorný). Komunikace s Filipem česky.

## Hosting a nasazení
- Repo: https://github.com/FilipSery/ai1st.cz (veřejné), větev `main`.
- Hosting: **GitHub Pages** z `main` / root, vlastní doména `ai1st.cz` (soubor `CNAME`), Enforce HTTPS zapnuté.
- Každý push na `main` nasadí web (workflow `pages-build-deployment`, obvykle do 1 minuty; jednou se po commitu nespustil a pomohl až další commit).
- Doména a DNS: **WEDOS** (DNS u WEDOS). Záznamy: 4× A `185.199.108–111.153`, 4× AAAA `2606:50c0:8000–8003::153`, `www` CNAME `filipsery.github.io.`. Žádné MX – e-maily na doméně zatím nejsou (zvažován WEDOS.email nebo Google Workspace; po zřízení doplnit SPF/DKIM/DMARC a na webu vyměnit kontakt za firemní adresu).

## Soubory
- `index.html` – celý web v jednom souboru: Tailwind přes CDN, GSAP 3.12.5 + ScrollTrigger z cdnjs, fonty Space Grotesk / Inter / JetBrains Mono.
- `zpracovani-osobnich-udaju.html` – zásady zpracování osobních údajů (čisté CSS, bez Tailwindu). Správce: Filip Šerý; IČO a sídlo zatím neuvedeny. Cookies lišta není, web cookies nepoužívá.
- `reklama/` – podklady pro Meta Ads (HTML zdroje, PNG snímky, MP4, fotky). **Necommitovat**: na GitHub Pages by byly veřejné.
- `CNAME`, `README.md`.
- `napojit-na-github.bat` – jednorázový skript na napojení složky na repo; po použití smazat (není součást webu).

## Struktura stránky (pořadí sekcí)
1. Hero – tichý canvas s částicemi (jedna barva, uhýbají myši), nadpis „AI na prvním místě. Rutina na posledním.“
2. Statický řádek technologií.
3. `#sluzby` – 3 ploché karty: B2B AI automatizace, OCR pipeline na míru, lokální pseudonymizace dat.
4. `#manifest` – připnutá sekce, slova se při scrollu rozsvěcují (pin, `refreshPriority: 2`).
5. `#pipeline` – 5 kroků s kolejnicí + demo pseudonymizace (`pseudonymizer.py`, vymyšlené údaje).
6. `#case` – připnutá scéna „Dokument dorazí. Zbytek se stane sám.“: sken → OCR → vytažení údajů → zápis do spisového systému (pin jen na ≥1024 px, `refreshPriority: 1`). Průběh je ilustrační odhad, ne skutečná zakázka; jméno klienta se na webu neuvádí bez jeho souhlasu.
7. `#architekti` – Filip a Vojta, místo fotek placeholdery s iniciálami (FŠ, VP). Vojtův popis je zatím obecný – Filip ho doplní později.
8. Pás „MÉNĚ RUTINY / VÍC VÝSLEDKŮ“ (protiběžný posun).
9. `#kontakt` – vlevo text a důvody, vpravo kontaktní formulář (jméno, e-mail, firma nepovinně, zpráva, skryté pole proti botům). Pod formulářem a v patičce odkaz na zásady.

## Známé problémy a pravidla
- **Dvě připnuté sekce:** ScrollTrigger musí počítat horní pin dřív (`refreshPriority`), jinak spodní scéna začne o výšku horní rezervy dřív a překrývá obsah. Při přidání dalšího pinu dát všem pinům prioritu shora dolů.
- Stránka po změně výšky přeměří triggery (`ResizeObserver` + `document.fonts.ready`), protože Tailwind z CDN a fonty mění layout až po načtení.
- CSS `scroll-behavior: smooth` je vypnuté (pere se se ScrollTriggerem); kotvy scrollují plynule přes JS.
- Scéna `#case` má obsah ~690 px – na oknech nižších než ~770 px se spodek může oříznout.
- Tailwind CDN vypisuje varování „should not be used in production“; plán: přepsat na čisté CSS nebo build přes Tailwind CLI.
- **Čistý vzhled (11/2026):** jeden akcent (mint `#34f5c5`), žádná fialová ani gradientový text, ploché karty `.card`, žádný vlastní kurzor, tilt, magnetická tlačítka, šum ani scroll-reveal na každé sekci. Pohyb zůstává jen: vstup hera při načtení, pin `#manifest`, kolejnice + demo v `#pipeline`, scéna `#case`, pás. Hover/press do 200 ms, `:active` scale .97, ease-out `cubic-bezier(.23,1,.32,1)` (v GSAP `expo.out`). Nové efekty nepřidávat bez důvodu.
- Skilly v `.claude/skills/`: impeccable, design-taste-frontend (taste skill), Emil Kowalski (`review-animations` a další). Formální `review-animations` ani gate taste skillu na webu zatím neběžely.

## Kontaktní formulář
- GitHub Pages neumí odesílat poštu. V `index.html` je konstanta `WEB3FORMS_KEY`: **prázdná = formulář jen otevře e-mailový program návštěvníka s předvyplněnou zprávou** (mailto). Po vložení klíče z web3forms.com se zpráva odešle přímo ze stránky.
- Po vložení klíče je nutné v zásadách (bod 5) nahradit odstavec „Formulář zprávu neodesílá přes žádnou další službu…“ zmínkou o Web3Forms jako zpracovateli.
- Při přidání analytiky nebo reklamních pixelů (Meta Pixel) upravit bod 6 zásad a řešit cookies lištu.

## Nápady / další kroky
- Skutečný popis automatizace pro klienta (vstupy, vytahovaná pole, cílový systém) → přepsat scénu `#case`.
- Fotky zakladatelů místo placeholderů.
- Firemní e-mail + DNS záznamy.
