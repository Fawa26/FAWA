# fawa.dk

Kildekoden til FAWAs hjemmeside. Ren HTML, CSS og JavaScript — ingen
byggeproces, ingen afhængigheder. Filerne kan åbnes direkte i en editor.

## Sådan er det skruet sammen

Hver side i menuen har sin egen mappe med en `index.html`. De er alle den
samme hjemmeside, men hver fil har sin egen titel, beskrivelse og
`canonical`-adresse, så Google kan indeksere dem hver for sig. Når man
klikker rundt, skifter JavaScript bare hvilken sektion der er synlig og
opdaterer adressen — der bliver ikke hentet en ny fil.

```
/                     forsiden
/ydelser/             ydelser og priser
/cases/               cases
/om/                  om os
/kontakt/             kontakt
/faq/                 ofte stillede spørgsmål
/privatlivspolitik/
/cookiepolitik/
/handelsbetingelser/
404.html              vises ved ukendte adresser
assets/img/           billeder (WebP)
assets/fonts/         skrifttyper (WOFF2, selvhostede)
assets/fonts.css      @font-face-erklæringer
assets/favicon.svg
og.jpg                billedet der vises når linket deles
sitemap.xml           listen Google læser
robots.txt
.nojekyll             beder GitHub Pages om ikke at behandle filerne
```

Sproget styres af `data-i18n`-attributter i HTML. Dansk står direkte i
markuppen; engelsk ligger i `EN`-objekterne i scriptet nederst i filen.
Valget gemmes i `localStorage` under `fawa-lang`.

## Når du retter noget

Retter du tekst eller design, skal ændringen ind i **alle ni
`index.html`-filer** — de er kopier af hinanden. Det nemmeste er at rette
i `index.html` i roden og derefter kopiere `<body>`-indholdet over i de
andre, eller bede om en ny generering.

Skrifttyperne er hentet fra Google Fonts og lagt lokalt, så der ikke
sendes data til Google, når nogen besøger siden. De er alle under SIL
Open Font License 1.1.

## Spærring mod søgemaskiner uden for fawa.dk

Øverst i hver `index.html` ligger fem linjers JavaScript, der beder Google og
andre søgemaskiner om at holde sig væk, hvis siden bliver vist på en anden
adresse end `fawa.dk` — for eksempel GitHubs midlertidige
`<brugernavn>.github.io`-adresse. På fawa.dk gør den ingenting.

Den slår altså sig selv fra, når domænet er på plads. Der er intet, I skal
huske at fjerne. Hver side peger desuden med `canonical` på sin rigtige
fawa.dk-adresse, og `sitemap.xml` nævner kun fawa.dk.

## Udgivelse på GitHub Pages

1. Opret et nyt repository på GitHub, for eksempel `fawa-dk`.
2. Læg indholdet af denne mappe i roden af repoet og push til `main`.
3. Gå til **Settings → Pages**. Vælg `Deploy from a branch`, branch
   `main`, mappe `/ (root)`. Gem.
4. Efter et par minutter ligger siden på
   `https://<brugernavn>.github.io/<repo>/`. Den virker fuldt ud dér — links,
   undersider og sprogskifter fungerer under enhver sti, så I kan tjekke det
   hele igennem, inden domænet bliver sat om.
6. Under **Custom domain** skriver I `fawa.dk` og gemmer — først når DNS er
   sat op (se nedenfor).
7. Sæt flueben i **Enforce HTTPS**, når certifikatet er klar. Det kan tage op
   til et par timer første gang.

## DNS hos one.com

Domænet ligger hos one.com, og det peger i dag på one.coms egne servere.
Det skal peges over på GitHub, ellers ved fawa.dk ikke, hvor hjemmesiden
ligger. Under DNS-indstillinger for fawa.dk skal der stå:

| Type  | Navn  | Værdi |
|-------|-------|-------|
| A     | @     | 185.199.108.153 |
| A     | @     | 185.199.109.153 |
| A     | @     | 185.199.110.153 |
| A     | @     | 185.199.111.153 |
| CNAME | www   | `<brugernavn>.github.io.` |

`<brugernavn>` er jeres GitHub-brugernavn. Slet eventuelle eksisterende
A-records på `@`, der peger på one.coms egne servere — ellers rammer
domænet det forkerte sted.

**Mailen bliver ikke berørt.** Rør ikke MX-records eller noget med
`mail`, `imap`, `smtp`, `autodiscover`, `_dmarc`, `_domainkey` eller
`spf`. Det er dem, der styrer contact@fawa.dk. Kun A-records på `@` og
CNAME på `www` skal ændres.

Sæt ikke DNS om, før hjemmesiden ligger på GitHub og virker på
`<brugernavn>.github.io/<repo>/`. Så er der ikke et vindue, hvor fawa.dk
ikke svarer. Der går typisk 15 minutter til et par timer, før ændringen
er slået igennem.

## Når siden er live

- Meld fawa.dk til Google Search Console og indsend `sitemap.xml`.
- Del linket ét sted og se, at billedet og teksten fra `og.jpg` kommer med.
- Kør siden gennem PageSpeed Insights.
