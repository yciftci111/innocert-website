# InnoCert — website

Statische website voor InnoCert, klaar om via GitHub te beheren en met GitHub Pages te publiceren.

**Elk `.html`-bestand is volledig zelfstandig**: stijl (CSS), interactiviteit (JS) en het logo zitten er allemaal in verwerkt. Er zijn geen aparte mappen (`css/`, `js/`, `assets/`) meer nodig — dat loste eerdere upload-problemen op waarbij die mappen niet meekwamen. Je kunt nu gewoon de 8 losse `.html`-bestanden uploaden en de site werkt direct.

## Structuur

```
index.html                  Homepage — tweezijdige propositie (opdrachtgevers + freelancers)
expertise.html              Vakgebied/expertise-overzicht
opdrachtgevers.html         Voor bedrijven/CI's die een specialist zoeken
freelancers.html            Voor freelance auditors — incl. aanmeldformulier (#aanmelden)
over-ons.html                Over InnoCert
contact.html                 Algemeen contactformulier
blog.html                    Blog-overzicht
blog-artikel-voorbeeld.html Sjabloon voor een los blogartikel — kopieer dit bestand voor elk nieuw artikel
```

Dat is alles — 8 bestanden, geen submappen.

## 1. Op GitHub zetten (eenvoudig, geen mappen nodig)

1. Maak een nieuwe **public** repository aan op GitHub, bijv. `innocert-website`.
2. Open de repository en klik op **Add file → Upload files**.
3. Sleep de 8 `.html`-bestanden (gewoon los, uit de uitgepakte zip) het uploadvak in — of klik op "choose your files" en selecteer ze allemaal tegelijk.
4. Klik **Commit changes**.
5. Ga naar **Settings → Pages**.
6. Bij **Branch**, kies `main` en map `/ (root)` → **Save**.
7. Na een paar minuten is de site bereikbaar op `https://<jouw-gebruikersnaam>.github.io/<repository-naam>/`.

Controleer na het uploaden of er in de bestandenlijst van de repo precies 8 `.html`-bestanden staan, zonder extra mappen — dan kan er niets meer ontbreken.

## 2. Eigen domein koppelen (innocert.nl)

Je huidige DNS-zone bij je provider bevat dit:

| Naam | Type | Content | Actie |
|---|---|---|---|
| `www` | A | 185.104.28.238 | **Vervangen** door CNAME → `<jouw-gebruikersnaam>.github.io` |
| `www` | AAAA | 2a06:2ec0:1::ffed | **Verwijderen** (vervalt samen met de A-record hierboven) |
| `@` | A | 185.104.28.238 | **Vervangen** door 4 A-records (zie hieronder) |
| `@` | AAAA | 2a06:2ec0:1::ffed | **Vervangen** door 4 AAAA-records (optioneel, zie hieronder) |
| `ftp` | A / AAAA | 185.104.28.238 / 2a06:2ec0:1::ffed | **Laten staan** — tenzij je geen FTP-toegang tot de oude hosting meer nodig hebt |
| `mail` | A / AAAA | 185.104.28.238 / 2a06:2ec0:1::ffed | **Niet aanraken** — anders werkt je e-mail niet meer |
| `@` | NS (3x, ns.zxcs.\*) | — | **Niet aanraken** |
| `_domainkey`, `_dmarc`, `@` (SPF) | TXT | — | **Niet aanraken** — belangrijk voor e-mailafzending/DKIM/DMARC/SPF |

### Stap voor stap

1. **Verwijder** de bestaande `www` A-record en `www` AAAA-record.
2. **Voeg toe**: `www` CNAME → `<jouw-gebruikersnaam>.github.io`.
3. **Verwijder** de bestaande `@` (kale domein) A-record.
4. **Voeg toe**: vier `@` A-records:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
5. *(Optioneel, IPv6)* **Vervang** de bestaande `@` AAAA-record door deze vier:
   ```
   2606:50c0:8000::153
   2606:50c0:8001::153
   2606:50c0:8002::153
   2606:50c0:8003::153
   ```
6. Laat `ftp`, `mail`, de drie `NS`-records en de drie `TXT`-records ongewijzigd.
7. In de GitHub-repository: maak een bestand **`CNAME`** (geen extensie, gewoon los uploaden net als de andere bestanden) met als inhoud:
   ```
   innocert.nl
   ```
8. In **Settings → Pages**: vul bij **Custom domain** `innocert.nl` in en sla op. Zodra GitHub de DNS heeft geverifieerd (kan tot 24-48 uur duren) verschijnt de optie **Enforce HTTPS** — vink die aan.
9. GitHub zorgt er dan automatisch voor dat `www.innocert.nl` doorverwijst naar `innocert.nl`.

## 3. Formulieren laten werken

Het contactformulier (`contact.html`) en het aanmeldformulier voor freelancers (`freelancers.html`) tonen nu alleen een bevestigingstekst, maar versturen nog niets. Eenvoudigste oplossing: **Formspree** (gratis voor laag volume).

1. Maak een gratis account aan op formspree.io en maak een formulier aan.
2. Open `contact.html` (en/of `freelancers.html`) en zoek de regel:
   ```html
   <form id="contact-form">
   ```
   Vervang door:
   ```html
   <form id="contact-form" action="https://formspree.io/f/JOUW-ID" method="POST">
   ```
3. Zoek in hetzelfde bestand het `<script>`-blok onderaan de pagina en verwijder daar de regel `e.preventDefault();` uit de submit-handler voor dat formulier, zodat de browser het formulier daadwerkelijk naar Formspree stuurt.

Laat het weten als je wilt dat ik dit direct voor je inregel.

## 4. Inhoud aanpassen

- Alle teksten staan direct in de HTML-bestanden — makkelijk te vinden en aan te passen met een teksteditor.
- Omdat alles zelfstandig is, moet je **stijlwijzigingen** (kleuren, lettertype) in elk bestand los aanpassen, of mij vragen dit centraal voor je te doen en opnieuw te genereren.
- Nieuwe blogartikelen: kopieer `blog-artikel-voorbeeld.html`, pas titel/tekst aan, en voeg een kaart toe op `blog.html` (en eventueel `index.html`).

## 5. Lokaal bekijken

Geen installatie nodig — dubbelklik een `.html`-bestand om 'm direct in je browser te openen.
