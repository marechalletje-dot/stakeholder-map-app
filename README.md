# Stakeholder Map — web app (PWA)

Een stakeholder-dashboard dat als app op iPhone, iPad en laptop draait. Je plaatst het op GitHub Pages, en daarna installeer je het via Safari met **Zet op beginscherm**.

## Privacy: lees dit eerst

- **De repository bevat géén klantdata.** Alle contactpersonen, organisatienamen en notities zijn verwijderd. De app start leeg.
- **Dashboards worden alleen lokaal opgeslagen**, in de browser van het apparaat (IndexedDB). Er gaat niets naar GitHub of naar een andere server.
- **Een GitHub Pages-site is openbaar** (behalve op GitHub Enterprise). Iedereen met de link kan de lege app openen, maar niemand ziet jouw dashboards.
- **Zet nooit opgeslagen dashboardbestanden (.html/.json met klantdata) in deze repository.**

## Online zetten (eenmalig)

1. Maak op github.com een nieuwe repository aan, bijvoorbeeld `stakeholder-map`.
2. Kies **Add file → Upload files** en sleep de *inhoud* van deze map erin:
   - `index.html`
   - `manifest.webmanifest`
   - `sw.js`
   - `.nojekyll`
   - de map `icons/`
3. Klik op **Commit changes**.
4. Ga naar **Settings → Pages**. Kies bij *Source* **Deploy from a branch**, met branch **main** en map **/ (root)**, en klik op **Save**.
5. Na ongeveer een minuut staat de app op `https://<jouw-gebruikersnaam>.github.io/stakeholder-map/`.

## Installeren op de iPhone

1. Open de link in **Safari** (niet in Chrome of de Teams-browser).
2. Tik op de deelknop en kies **Zet op beginscherm** → **Voeg toe**.
3. De app opent daarna schermvullend met het eigen icoon en werkt ook offline.

## Je bestaande dashboards overzetten

- Zet het bewaarde `.html`-dashboard via OneDrive, AirDrop of mail in de app **Bestanden** op je iPhone.
- Open de app, tik op **Open saved dashboard…** (of **Dashboards → Open file…**) en kies het bestand.
- Het dashboard staat daarna in de app en wordt automatisch bijgehouden. Je kunt meerdere klanten naast elkaar beheren via **Dashboards**.

## Delen en back-up

- **Export file** maakt een los `.html`-bestand van het open dashboard. Collega's openen dat zoals vroeger op hun laptop. Op de iPhone opent het deelmenu: *Bewaar in Bestanden*, Mail, Teams, enzovoort.
- **Dashboards → download a backup of all dashboards** maakt één `.json`-back-up van alle dashboards. Die kun je via **Open file…** weer terugzetten.
- Maak regelmatig een back-up. De gegevens staan alleen op dit apparaat: verwijder je de app of wis je de Safari-websitegegevens, dan zijn ze weg.

## Nieuwe versie uitbrengen

1. Vervang `index.html` in de repository.
2. Verhoog `VERSION` in `sw.js`, bijvoorbeeld `sm-v1.0.1`.
3. Commit. Geïnstalleerde apps halen de nieuwe versie op zodra ze online zijn geopend.

## Architectuur in het kort

| Onderdeel | Wat het doet |
|---|---|
| `index.html` | De volledige app: UI, Excel-import (SheetJS), PNG/PDF-export (html2canvas, jsPDF), allemaal inline. Daardoor blijft een geëxporteerd dashboard een zelfstandig bestand. |
| IndexedDB `stakeholder-map-app` | Lokale bibliotheek met dashboards. Elke wijziging wordt automatisch opgeslagen. |
| `sw.js` | Service worker: de app werkt offline. De pagina wordt eerst van het netwerk gehaald, zodat updates snel binnenkomen. Er wordt nooit dashboarddata gecachet. |
| `manifest.webmanifest` + `icons/` | Appnaam, kleuren en iconen voor het beginscherm (iOS en Android). |

**Twee modi in één bestand:**

- **App-modus** (geopend via https): lokale bibliotheek met automatisch opslaan.
- **Bestandsmodus** (los `.html`-bestand op een laptop): werkt precies zoals voorheen, met *Save dashboard* en *Save as…*.
